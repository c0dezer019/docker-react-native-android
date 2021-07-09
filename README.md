# React Native Android Base

A version of [Dockerfile.android-base](https://github.com/facebook/react-native/blob/988366a4179d87d667e5d9396efdfba4cbbe0b2e/ContainerShip/Dockerfile.android-base) from the React Native repo.

***

## **Usage**

### **Adding a user**

By default, the system image creates a user named 'user' and only installs android-30 as a platform. If you'd like to create your own user than use the following command:

`$ docker run -it react-native-android-base bash`

Wait for it to open the prompt (you're now in the container bash shell) and run the useradd command, basic usage is as follows (the portion in bracket is optional):

`$ useradd [-r] [-s /bin/bash] username [-p password]`

### **Adding more Android SDK Packages**

If you want to add more Android platforms or sdk packages, here's an example command on how to do that:

`$ sdkmanager "platforms;android-29" "platforms;android-27" "system-images;android-S`

### **Rebuilding**

If you need or want to rebuild the image, you can setup a default platform and user by passing `--build-arg user=user --build-arg build_version=version` to the build command.

***

## **Running a React Native Project**

The basic idea is to extend the Docker image with another written explicitly for your project.
