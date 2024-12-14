=======
Node.js
=======

* `What is Node.js`_
* `Globals`_
* `Modules`_
* `Event Loop`_
* `Event`_
* `Streams`_
* `HTTP`_
* `Express`_
* `HTTP Methods`_
* `JSON Web Token`_

What is Node.js
===============

* environment to run JS outside Browser, built on Chrome's V8 JS Engine
* no DOM & Window
* used to build server side apps

`back to top <#top>`_

Globals
=======

* ``__dirname`` : path to current dir
* ``__filename``
* ``require`` : function to use modules (CommonJS)
* ``module`` : info about current module (file)
* ``process`` : info about env where the program is being executed

`back to top <#top>`_

Modules
=======

* every file is module by default
* encapsulated code (only share minimum)

    .. code-block:: js

       module.exports = { modules to export }
       const x = require('./moduleName')


* Alternatively

    .. code-block:: js

       module.exports.moduleToExport = value


* Built-in Modules
    * OS

        .. code-block:: js

           const os = require('os')
           os.userInfo()
           os.release()


    * PATH

        .. code-block:: js

           const path = require('path')
           path.basename(filePath)
           // concatenate with previous argument, /dir/subfolder/file
           path.join('/dir', 'subfolder', 'file')
           path.resolve('/dir', '/subfolder', 'file') // treat as root directory, /subfolder/file


    * FS

        .. code-block:: js

           // sync
           const { readFileSync, writeFileSync } = require('fs')
           readFileSync(path, encoding) // generally 'utf8'
           writeFileSync(path, value, {flag: 'a'}) /* omit flag will create if not exist or overwrite,
                                                      otherwise append */
   
           // async
           const { readFile, writeFile } = require('fs')
           readFile(path, encoding, callBack) // can chain multiple readFile, writeFile within callBack
           writeFile(path, encoding, callBack, {flag: 'a'}) // (err, result) => {}
   
           // using this read n write use more memory as it manipulates the whole file


    * HTTP

        .. code-block:: js

           const http = require('http')
           const server = http.createServer((req, res) => {
             res.writeHead(200, {'content-type': 'text/html'}) //send header, content-type matters
             res.write()
             res.end()
             if(req.url === '/'){
               res.end('')
             }
           })
   
           // OR
           const server = http.createServer()
           server.on('request', (req, res) => {})
   
           server.listen(port)


    * UTIL

        .. code-block:: js

           const util = require('util')
           const readFilePromise = util.promisify(readFile)
           const writeFilePromise = util.promisify(writeFile)
           readFilePromise(path, encoding) // returns promise


* npm
    * npm install / yarn install (to install dependicies from package.json)
    * npm i / yarn add -D / --save-dev

`back to top <#top>`_

Event Loop
==========

* JavaScript is Synchronous and Single Threaded. We can offload time consuming synchronous
  tasks with Event Loop.

`back to top <#top>`_

Event
=====

* many built-in modules rely on concept of events (e.g http)

    .. code-block:: js

       const EventEmitter = require('events')
       const customEmitter = new EventEmitter()
   
       // can have multiple on and emit, ordering matters
       customEmitter.on('event', (arg1, arg2) =>{} ) // on - listen an event
       customEmitter.emit('event', arg1, arg2) // emit - emit an event


`back to top <#top>`_

Streams
=======

* used to read or write sequentially, handy when needed to handle streaming data, continuous
source or big files
* writable
* readable
* duplex (read n write)
* transform

.. code-block:: js

   const { createReadStream } = require('fs')
   // default highWaterMark 64KB
   const stream = createReadStream('path', {highWaterMark: value, encoding: 'utf8'})
   stream.on('data', (result) => {}) // will read data in chunks
   stream.on('error', () => {})
   
   // manipulating data in chunks, instead of one large instance, is more efficient
   http.createServer((req, res) => {
     const stream = createReadStream('path', 'utf8')
     stream.on('open', () => {
       stream.pipe(res) // pipe push from readStream into writeStream
     })
     stream.on('erro', (err) => {
       res.end(err)
     })
   }).listen(5000)


`back to top <#top>`_

### HTTP
* Request Message - sent by clients
    * default method is GET
    * Method, URL, HTTP version, **Headers**, **Body**
        - Headers are, key-value pairs, metainformation about the message (optional)
        - Body is optional if you just want the resource, but needed if to add the resource
        onto the server (payload)
* Response Message - sent from server
    * have to do manually
    * HTTP version, [**Status Code**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status), Status Text, Headers, Body
        - status code signals the result of the request
    * send back text/html and application/json
        - APIs send back json data

HTTP methods
------------
    * GET    /Read Data
    * POST   / Insert Data
    * PUT    / Update Data
    * DELETE / Delete Data
* `Ports`_
* `MIME types`_
* example http app

    .. code-block:: js

       const http = require('http')
       const { readFileSync } = require('fs')
   
       // this will only give index.html file, not others such as css and scripts
       const myApp = readFileSync('./app/index.html')
       // have to readFile all resources if want to
   
       const server = http.createServer((req, res) => {
       if (req.url === '/') {
         res.writeHead(200, { 'content-type': 'text/html' })
         res.write(myApp)
       }
       else if (req.url === '/about') {
         res.writeHead(200, { 'content-type': 'text/html' })
         res.write('<h1>about page</h1>')
       }
       else {
         res.writeHead(400, { 'content-type': 'text/html' })
         res.write('<h1>page not found</h1>')
       }
       res.end()
       })
   
       server.listen(5000)


`back to top <#top>`_

Express
=======

* web framework for Node
* less code than using built-in http module

.. code-block:: js

   const express = require('express')
   const app = express()
   // get, post, put, delete, all, use, listen
   
   app.get('/', (req, res) => {
     res.send()
   })
   
   app.all('*', (req, res) => {
     res.status(STATUS_CODE).send('res for all') // can set status for each send
   })
   
   app.listen(PORT, () => {})


.. code-block:: js

   // http app from above in express
   const express = require('express')
   const app = express()
   const path = require('path')
   
   // put all static files including index.html, public name by convention
   app.use(express.static('./public'))
   
   app.listen(5000, () => {console.log('server on port 5000')})


* static
    * assets server doesn't need to change
* API
    * JSON, Send data, res.json()
* SSR
    * Template, Send template, res.render()
* res.json()
    * send data converted to JSON string using JSON.stringify()
* route parameters

    .. code-block:: js

       // productID as a placeholder
       app.get('/api/products/:productID', (req, res) => {
       // req.params is object
       const {productID} = req.params
       // productID has string value
       const singleProduct = products.find((product) => product.id === Number(productID))
       // will prevent sending 200 if undefined
       if (!singleProduct) {
         return res.status(404).send('Product Not Found')
       }
       res.json(singleProduct)
       })


* query string/url parameters
    * send small amount of data using url
        - http://hn.algolia.com/api/v1/users/:username
        - http://hn.algolia.com/api/v1/search?query=foo&tags=story
          + everything after ? is not part of url and is sent to server to decide what to do
          + will send {query: 'foo', tags: 'story'}

        .. code-block:: js

           app.get('/api/v1/query', (req, res) => {
           // req.query is object
           const { search, limit } = req.query
           let sortedProducts = [...products]
           if (search) {
           sortedProducts = sortedProducts.filter((product) => {
           return product.name.startsWith(search)
           })
           }
           if (limit) {
           sortedProducts = sortedProducts.slice(0, Number(limit))
           }
           if (sortedProducts.length < 1) {
           return res.status(200).json({ success: true, data: [] })
           }
           res.status(200).json(sortedProducts)
           })


* middleware
    * function that execute during the request
    * has access to request and response objects
    * req => middleware => res
    * always pass to next unless terminating the whole cycle by sending back the response

    .. code-block:: js

       // middleware function
       const logger = (req, res, next) => {
       // code
       next()
       }
       // OR
       const logger = (req, res, next) => {
       // code
       res.send() // terminate
       }
   
       // express auto pass req,res,next to middleware
       app.get('/', logger, (req, res) => {
       res.send()
       })


    .. code-block:: js

       // better way
       app.use(logger) // apply all routes
       app.use('/api', logger) // will only apply routes after /api/
       app.get('/', (req, res) => {})


    .. code-block:: js

       app.use([middleware_1, middleware_2]) // order matters
       app.get('/', [middleware_1, middleware_2], (req, res) => {}) // can also add to routes


    .. code-block:: js

       app.use(express.static('./public')) // express built-in middleware


    .. code-block:: js

       const morgan = require('morgan') // third-party middleware
       app.use(morgan('tiny'))


* router
    * setup base router in main (like app.js)
    * routes in other files just have to use router.METHOD('/', (req, res) => {})
    * by convention, all other routes are put in folder "routes"

    .. code-block:: js

       //example route
       // in routes.js
       const router = express.Router()
       router.get('/')   // "/api/"
       router.post('/:id')   // "/api/:id"
       router.put('/:id')
       router.delete('/:id')
       modules.export = router
   
       // in app.js, base route
       const routes = require('./routes')
       app.use('/api', routes)
       // app.use('/other_routes', modules)


* controllers
    * splitting routers and functions
    * by convention, all files are put in folder "controllers"

    .. code-block:: js

       // in routes/routes.js
       const {function1, function2, function3} = require('../controllers/file.js')
       router.get('/', function1)
       router.post('/:id', function2)
       router.put('/:id', function3)
   
       // OR chain same routes
       router.route('/').get(function1)
       router.route('/:id').post(function1).put(function3)


`back to top <#top>`_

HTTP Methods
============

* GET (read)
    * www.site.com/api/orders, get all orders
    * www.site.com/api/orders/:id, get single order (path params)
    * app.get
* POST (insert)
    * www.site.com/api/orders, place an order (send data)
    * cannot configure browser to perform post request

    .. code-block:: js

       // parse form data
       app.use(express.urlencoded({ extended: false }))
       app.post('/login', (req, res) => {
       const { name } = req.body
       if (name) {
         return res.status(200).send(`Welcome ${name}`)
       }
       res.status(401).send('Please Provide Data')
       })
       // request content-type: application/x-www-form-urlencoded


    .. code-block:: js

       // parse json
       app.use(express.json())
       app.post('/api/people', (req, res) => {
       const { name } = req.body
       if (!name) {
         return res.status(400).json({ success: false, msg: 'please provide data' })
       }
       res.status(201).json({ success: true, person: name })
       })
       //request content-type: application/json


    * above methods trouble when have to setup multiple routes
    * using [**Postman**](https://www.postman.com/)
        - faster to test without building frontend
* PUT (update)
    * www.site.com/api/orders/:id, update specific order (params + send data)
    * app.put
    * replace the existing resource
    * **PATCH**
        - partial update
        - update only data that was passed in
* DELETE (delete)
    * www.site.com/api/orders/:id , delete order (path params)
    * app.delete

`back to top <#top>`_

JSON Web Token
==============

* a way to exchange data between two parties (frontend & api)
* has a security feature for integrity of data (more on [jwt.io](https://jwt.io/))
* Login Request, Response + Signed JWT
    * if user provide correct credentials, signed jwt is sent back
* Request + Signed JWT, Response
* Structure
    * three parts separated by dots
        - Header
        - Payload
        - Signature

.. code-block:: js

   //example using jsonwebtoken package
   const jwt = require('jsonwebtoken')
   
   // provide payload, do not send confidential data, keep as small as possible
   // keep JWTSECRET secure
   const token = jwt.sign({DATA}, process.env.JWTSECRET, {expiresIn:'30d'})
   
   // decoding
   const decoded = jwt.verify(token, process.env.JWTSECRET)


`back to top <#top>`_
