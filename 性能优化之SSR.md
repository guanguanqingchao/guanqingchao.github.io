参考：
- https://juejin.im/post/5c92f499f265da612647b754#heading-9
- https://github.com/alexnm/react-ssr
- https://juejin.im/post/5af443856fb9a07abc29f1eb

### SSR(服务端渲染) 

- 首屏渲染
- seo


#### 原理:

Node 服务: 让前后端运行同一套代码成为可能。
Virtual Dom: 让前端代码脱离浏览器运行。

#### 条件: Node 中间层、 React / Vue 等框架。 

#### 本文以react redux react-router express为例 

- 项目结构

        build
        public
        src
            client.js
            server.js
        
     
 #### server.js
 
    import express from "express";
    import path from "path";

    import React from "react";
    import { renderToString } from "react-dom/server";
    import { StaticRouter, matchPath } from "react-router-dom";
    import { Provider as ReduxProvider } from "react-redux";
    import Helmet from "react-helmet";
    import routes from "./routes";
    import Layout from "./components/Layout";
    import createStore, { initializeSession } from "./store";

    const app = express();

    app.use( express.static( path.resolve( __dirname, "../dist" ) ) );

    app.get( "/*", ( req, res ) => {
        const context = { };
        const store = createStore( );

        store.dispatch( initializeSession( ) );

        const dataRequirements =
            routes
                .filter( route => matchPath( req.url, route ) ) // filter matching paths
                .map( route => route.component ) // map to components
                .filter( comp => comp.serverFetch ) // check if components have data requirement
                .map( comp => store.dispatch( comp.serverFetch( ) ) ); // dispatch data requirement

        Promise.all( dataRequirements ).then( ( ) => {
            const jsx = (
                <ReduxProvider store={ store }>
                    <StaticRouter context={ context } location={ req.url }>
                        <Layout />
                    </StaticRouter>
                </ReduxProvider>
            );
            const reactDom = renderToString( jsx );
            const reduxState = store.getState( );
            const helmetData = Helmet.renderStatic( );

            res.writeHead( 200, { "Content-Type": "text/html" } );
            res.end( htmlTemplate( reactDom, reduxState, helmetData ) );
        } );
    } );

    app.listen( 2048 );


    function htmlTemplate( reactDom, reduxState, helmetData ) {

        return `
            <!DOCTYPE html>
            <html>
            <head>
                <meta charset="utf-8">
                ${ helmetData.title.toString( ) }
                ${ helmetData.meta.toString( ) }
                <title>React SSR</title>
            </head>

            <body>
                <div id="app">${ reactDom }</div>
                <script>
                    window.REDUX_DATA = ${ JSON.stringify( reduxState ) }
                </script>
                <script src="./app.bundle.js"></script>
            </body>
            </html>
        `;
    }
    
#### client.js
        import React from "react";
        import ReactDOM from "react-dom";
        import { BrowserRouter as Router } from "react-router-dom";
        import { Provider as ReduxProvider } from "react-redux";

        import Layout from "./components/Layout";
        import createStore from "./store";

        const store = createStore( window.REDUX_DATA );

        const jsx = (
            <ReduxProvider store={ store }>
                <Router>
                    <Layout />
                </Router>
            </ReduxProvider>
        );

        const app = document.getElementById( "app" );
        ReactDOM.hydrate( jsx, app );
        
#### 路由 

 - 通过renderToString将html转换成字符串，渲染函数为ReactDOM.hydrate。该函数将接收已由服务端渲染的 React app， 并将附加事件处理程序。
 - React 应用外包一层StaticRouter，并且给StaticRouter提供location。
 
#### 数据redux
 - 调用了两次createStore，将数据序列化之后挂载到window到，client通过window获取来初始化
 - Server 层 数据脱水: 为了把服务端获取的数据同步到前端。主要是将数据序列化后，插入到 html 中，返回给前端。
 - Client 层 数据吸水: 初始化 store 时，以脱水后的数据为初始化数据，同步创建 store
 
 #### Fetch Data
 
 - Redux 首先在服务端上存储数据，再将数据发送客户端,
 - 在服务端上进行 API 调用，将结果存储在 Redux 中，然后使用再渲染携带着相关数据的完整的 HTML 渲染给客户端
 - 如何分辨某次 API 调用对应的是什么页面
 
         class Home extends React.Component {
            componentDidMount( ) {

                if ( this.props.circuits.length <= 0 ) {
                    this.props.fetchData( );
                }
            }

            render( ) {
                const { circuits } = this.props;

                return (
                    <div>
                        <h2>F1 2018 Season Calendar</h2>
                        <ul>
                            { circuits.map( ( { circuitId, circuitName, Location } ) => (
                                <li key={ circuitId } >{ circuitName } - { Location.locality }, { Location.country }</li>
                            ) ) }
                        </ul>
                    </div>
                );
            }
        }
        Home.serverFetch = fetchData; // static declaration of data requirements
 
 #### Helmet
 
 - 通常情况下<head>标签并不属于 React app 的一部分
 - 在组件树中的任意位置添加head数据。这使你可以在客户端上更改已挂载的 React app 以外的值。
    
        import Helmet from "react-helmet";
        const Contact = () => (
            <div>
                <h2>This is the contact page</h2>
                <Helmet>
                    <title>Contact Page</title>
                    <meta name="description" content="This is a proof of concept for React SSR" />
                </Helmet>
            </div>
        );

        export default Contact;
        
        




