# Complex : Building a Multi-container Application

In this project, we have to create a 'fancy fibonacci sequence' calculator application. We have called this "complex" since we're knowingly going to make it over-the-top complicated for us to understand `Multi-Container Deployment` 

To put it simply, there are, in general (for this project), 6 components, namely `Nginx server` , `React Server` , `Express Server` , `Worker` , `Redis` & `Postgres` . 
SInce we are making a Fibonacci generator, the general work of the following for this project are : 
1. Redis : Stores all indices and calculated values.
2. Postgres : Stores the permanent list of indices that have been received.
3. Worker : - watches redis for new indices
            - pulls the new indices
            - calculates the appropriate fibonacci number and sends back to redis.
4. Express Server : - serves as an API layer (sort of) which communicates with Redis & Postgres 
                    - communicates info over to the running React App.
                    
                
## Dockerizing Multiple Services 
We need to make Dev Dockerfile (Dockerfile.dev) for the React App, Express Server & Worker.

The Dockerfile contains :
  - copy package.json
  - Run npm install
  - copy over everything else
  - docker comppose should set up volumes to 'share' files

Adding Postgres as a service : (compose file)
  - Postgres
    - What image to use?
  - Redis
    - what image to use?
  - Server
    - specify build
    - specify volumes
    - specify envir. variables

***Nginx Path Routing***
In the last project, the nginx server was used to host all production files. We're taking care of the development envir. by nginx.
- here we are avoiding the pain in the butt of assigning & remembering the ports. Having a common backend to do the task is a better und feasible option. 

****steps**** :
1. Tell nginx there is an 'upstream' server at client:3000
2. Tell nginx there is an 'upstream' server at server:5000
   - Listen to Port 80
   - If someone comes to '/', send them to client upstream
   - If someone comes to '/api', send them to server upstream
   
   
Something about `webstock connections` and routing rules. 

## A continuous Integrated Workflow for Multiple Images

**Multi-container Setup**
1. Push code to Github (du-uh)
2. Travis automatically pulls repo
3. Travis builds a test image & tests the code
4. Travis builds production image
5. Travis pushes built prod image to Docker Hub
6. Travis pushes project to AWS EB (Elastic Beanstalk)
7. EB pulls image from Docker Hub, Deploys.

then we make the Dockerfile for Production env

***multiple nginx instances***
#### Development
Browser will make a request into the initial nginx server and this nginx server is specifically responsible for ROUTING! and making sure that the requests get to the correct backend.
#### Production
When we move into production, we have to take the 'port 3000' into account. Here as well, we have the nginx server solely dedicated to serving up our production react files, but rather than listening on port 80 & being the 'first jump' inside EB instance, it is going to be listening on port 3000. Users are only going to access this nginx server here with all our production files by first going through the other copy of nginx that is specifically responsible for Routing! .
We have to do a little bit of configuration of the nginx server, other than like last time (the project), to make sure that it listens to port 3000 instead, and still exposes all react production assets on port 3000. 



   

   
