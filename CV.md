# CV

## PyCharm Docker Setup

When asking for assistance with setting up PyCharm to work with the provided `docker-compose.yml` (actually it was `docker_compose.yml` which didn't work), most responses were just a link to the PyCharm documentation. This would make sense if those pages didn't go through a bunch of extra steps for their "example," which is nothing like what we needed for the class. Finally, I figured out that all I really needed to do was set up a Docker Compose Python interpreter. Here's all I had to do.

## Step 1: Fix/Edit the `docker_compose.yml` file

### Rename the file to `docker-compose.yml`

This one is self-explanatory.

### Edit the file (add "services")

For each project, you just add a new service under `services:` and change the path in `volumes:` to point to wherever the new project is. _DISCLAIMER: I really don't know for sure what every part of this file describes - some of it is understandable - so I'm just going with whatever I found on Piazza and the web._

```yaml
version: "3.3"
services:

  ps00:
    image: cv_all_image:latest
    volumes:
      - ../../PS/ps00:/home/pset
    environment:
      - NVIDIA_VISIBLE_DISPLAYS=all
      - DISPLAY
    user: 1000:1000

  ps01:
    image: cv_all_image:latest
    volumes:
      - ../../PS/ps01:/home/pset
    environment:
      - NVIDIA_VISIBLE_DISPLAYS=all
      - DISPLAY
    user: 1000:1000
```

## Step 2: Add a Docker Compose project interpreter

(I'll add images and clean this up later)

First, open the settings window. I'm on a Mac so the settings are at PyCharm > Preferences..., then open the Project: <name> drop-down menu and select Project Interpreter. There will be an icon near the top right of the screen that looks like your typical "settings" icon (a gear), click that and select "Add...." On the left side of the new window that pops up, select Docker Compose. There are three things you have to fill out, Server, Configuration file(s), and Service. The other two (Environment variables, and Python interpreter path) can be left alone. For Server, select New..., give it a name, and select Docker for Mac, and click OK. For Configuration files, click the folder icon on the right side of that field, and browe to the docker-compose.yml file. Finally, select a service that you defined in the docker-compose.yml file.

That's it. Now you can open a Python file, like ps0.py, and run/debug it as you normally would.

## Step 3: Create a Python debug configuration for each project

First, I think you have to create a new Project Interpreter for each project, and just select a different service from the same docker-compose.yml file (there might be a better way to do this - I don't know).
