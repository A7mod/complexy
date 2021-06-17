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


