# A Simple Flask app that has been dockerized.
## This example demonstrates how to make a **Dockerfile**

**Disclaimer:** The content is not of my own making.
Get a certificate from IBM via their Docker Essentials and more https://courses.cognitiveclass.ai/courses/course-v1:IBMDeveloperSkillsNetwork+CO0101EN+v1/.

This is just my own way of note-taking.

Steps
1. Create the app.py simply as:
    ```python

    from flask import Flask

    app = Flask(__name__)

    @app.route("/")
    def hello():
    return "hello world!"

    if __name__ == "__main__":
    app.run(host="0.0.0.0")
    
    ```

2. Create the Dockerfile as:
    ```
    FROM python:3.6.1-alpine
    RUN pip install flask
    CMD ["python","app.py"]
    COPY app.py /app.py
    ```
    * **FROM** is the starting point of very Dockerfile. It is the base layer on which you will build on top of. Try to choose a small and vetted image in sources such as the [Docker store](https://store.docker.com/)
    * **RUN** executes commands that are neccessary to set up the image for your application such as installing packages. **RUN** command is executed at build time.
    * **CMD** is the __command__ to start your application.
    * **COPY** copies the file from local directory to the new layer that will be built.

3. Create an Image:
    ``` 
    docker image build -t python-hello-world .
    ```
    ![Docker image build](https://github.com/Adrianteri/docker-practice/blob/master/images/Screenshot from 2019-01-06 19-36-39.png)
    ```
    docker image ls 
    ```
    ![Docker image ls](https://github.com/Adrianteri/docker-practice/blob/master/images/Screenshot from 2019-01-06 19-39-18.png)
    
4. Running the image:
    ```
    docker run -p 5001:5000 -d python-hello-world
    ```
    * the 5000 denotes the port for the container which is mapped to the local machine.

5. Check logs of container:
    ```
    docker container logs [container id/ name]
    ```
   ![Docker container logs](https://github.com/Adrianteri/docker-practice/blob/master/images/Screenshot from 2019-01-06 19-43-33.png) 
6. Push to central registry:
    ```
    docker login - Create an account if you don't have one at https://hub.docker.com/

    docker tag python-hello-world [username]/python-hello-world

    docker push [username]/python-hello-world
    ```
    ![Docker push](https://github.com/Adrianteri/docker-practice/blob/master/images/Screenshot from 2019-01-06 20-17-06.png)    

7. Deploy a change:
    update app.py
    ```python
    from flask import Flask

    app = Flask(__name__)

    @app.route("/")
    def hello():
        return "Hello Beautiful World!"


    if __name__ == "__main__":
        app.run(host='0.0.0.0')
    ```
    **Rebuild the image**
    ```
    docker image build -t adrianteri/python-hello-world .
    ```
    **Push** 
    ```
    docker push adriateri/python-hello-world
    ```
    * The caching mechanism is also in pushing layers. Docker hub already has layers from the earlier push.


