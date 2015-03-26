#Class Notes
##Basic





##Morden web development landscape
- Frontend
 - javascript : Coffeescript ,backbone....
 - HTML
 - CSS : SASS
 - mobile
- MiddleWare
  - API
  - URL
  - REST
  - request/response processing
  - security : xxs, crsf, code injection
- Backend
  - relational db ; Mysql Postgres
  - nosql db: mongodb
- WebServices
  - REST
  - SOAP

- MISC
  - deployment
  - background processing
  - git
  - bundler
  - rvm
  
 




##Sinatra
- The URL parameter is store in `params` hash in Sinatra, use it by `params[:key]`
- The instance varaible is for per request it's not persistent
- Session in Sinatra is like a hash, so you can use it by `session[:key] = value`
- You can reference **session**, **instance variable**, **helper method** in the vew template.
- In web development, make sure to know where you are in the MVC diagram
http://d1b1wr57ag5rdp.cloudfront.net/web_solutions/sinatra_mvc_request_response.pdf 
- **Helper method** can be used through out the entire application
- **render template** **will not** generate a new request ex:`erb :game`, but redirect will gernerate a new request, ex:`redirect '/game'`
- In main.rb use instance variable to comunicate with the view template, ex`@some_vairable`, notice that: **Instance variable is persist for just on request**
- Any thing declare in **before filter** will be automatically execute at the start of every action in main.rb