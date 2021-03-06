# Info pipeline


## How to run the software
Requirements: 
* Java
* Maven 
* Mongodb running on localhost:27017

Build the jar: 

    mvn clean package
    
Run the app: 
    
    java -jar target/infopipeline-0.0.1-SNAPSHOT.jar 
    
Then you can use a basic curl command to call the different endpoints

    curl  -F zipFile=@patents.zip http://localhost:8080/import 

    curl -X POST http://localhost:8080/db/drop
    

## Design decisions

Java vs python, I am more comfortable with java thus I choose Java.
I went with springboot for ease of use, and production ready features. We could also have looked at other frameworks such as micronaut for example.
Spring provides and easy way to build the rest controllers needed for the API, supports Mongo via spring data (could also be easy to switch to another database using JPA as intermediate and then just switching implementations of repositories)

The zipfile for the patents is uploaded as multipartFile.
The endpoint for dropping the database is POST as we want to specify that we are taking an action.
Both endpoints could have been defined as contract first with OpenAPi with Swagger extension, as a mean of documenting the API.
 
Parsing the xml is done with a DOM parser, with the use of Xpath expressions to retrieve the elements.
Also we use JAXB mappings to extract the application-reference part as an object.
I choose this approach because judging from the size of the files there is no problem in parsing them to memory and it is easier to manipulate, if that was a problem we could try using Stax (streaming) for example.
Also the code is structured in such way that we could switch the parser implementations if we want to try another library.

Inserts in Mongo are done using the java objects.

Depending on the use cases we could use an xsd schema from the patents and do mappings to Java Objects via JAXB, which would allow us to work on the model more easily.

The persist part should have its own model to be able to persist according to the needs of the other applications that may use this data.

For Ner part we used Standford NLP as suggested. We only used the CoreNLP Simple API because it fitted all our requirements. A more detailed analysis of the patents might require using the full API.
For now we persist the NE for each xml file, we could add more info if we had more requirements, or even reorganize and persist by zipfile, or deduplicate, or count occurences of each NE, etc.


## Scalability and maintainability
()changes in data volumes and formats
