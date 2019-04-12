First create a file named `Dockerfile` and add your commands.
See `cat Dockerfile`.

Then build your image with:

`docker image build --tag example1 .`

Now you can run the new image with:

`docker container run example1`
