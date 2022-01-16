# DeAI Project report Autumn 2021

---

**Author**: Lucas Giordano

**Supervisor**: Prof. Martin Jaggi

**Date**: January 14, 2022

Machine Learning and Optimization Laboratory (MLO), SC Section, EPFL

# Abstract

[DeAI](https://github.com/epfml/DeAI/) is a collaborative Machine Learning platform fully running in the browser. It delivers both decentralised and federated schemes in the same web application.

The current data-driven context imposes multiple limitations in the level of privacy and scalability achievable in training machine learning models.

Collaborative learning has progressively shown itself to be a formidable player in the process of addressing these concerns and will, undeniably, continue to be in the future.

An alpha version was deployed in spring 2021. It has provided the system foundations in terms of functionality and design. This project aims to improve the overall platform, from a system engineering perspective as well as incorporate additional features.

> Keywords: `machine learning`, `decentralised`,`federated`, `system design`, `open-source`, `JavaScript`.

# Background

A tremendous effort was made to build the alpha of the `DeAI` web platform in a single semester. If most of the individual components seemingly appeared to work perfectly, this rapid development has left the platform in an unstable state. Furthermore, the generated code was not aligned with academic and industry standards whether in organisation or modularisation.

Additionally, two side projects, `DeAI` and `FeAI` which had the same code base initially, were separated into two independent repositories which diverged from week to week. This choice has caused multiple problems for synchronisation and maintenance.

Finally, the `DeAI` project is carried out in an extremely dynamic environment in which students usually do not contribute for more than 4 months. The development team is therefore renewed each semester. The initial unstructured and hard-to-read code base constitute a barrier for the onboarding of new students, especially if they are not familiar with web development upon joining the project.

Consequently, there was a strong need for increasing readability and maintainability. I, along with a fellow student, Hugo El Guedj, took the lead in performing a global reshaping of the platform, introducing additional features along the way.

# Objectives

Through this maintenance project, 3 key objectives are addressed:

1. Modularisation
1. Additional features
1. Readability

This report shall describe a detailed list of the contribution I made throughout the semester, in chronological order. The following structure is adopted :

1. Motivation
2. Implementation details
3. Challenges and feedback

Finally, I will conclude and give several recommendations for the next steps of the project.

# Contributions

## Task Servers ([PR#136](https://github.com/epfml/DeAI/pull/136))

### Motivation

The communication topology is handled by `peerjs`. We do not have any control over the topology which is this case fully connected.

A single `Peerjs server` is constructed implying a single topology across all tasks. It is an inherent problem because there is an edge between each worker of `task A` and `task B`. In other words, they exchange their local gradients which a completely incorrect behaviour.

### Implementation details

To solve this issue, I proposed to associate a single `Peerjs server` for each task. Therefore, each constructed sub-server leverages its own independent topology.

Conceptually, we need to allocate as many ports (on the global server) as `peerjs sub-servers` for them to listen on requests from `DeAI` users. However, the number of ports that can be made publicly available is limited under the `Google App engine` subscription. To limit the scalability costs for MLO, we decided to develop a `reverse proxy` mechanism in order to use a single port which internally reroutes requests to the corresponding local private port.

> **Packages installed**: `http-proxy`, `http-proxy-middleware`, `lodash`

## New task creation form ([PR#165](https://github.com/epfml/DeAI/pull/165))

### Motivation

The following tasks are currently available for training : [Titanic](https://www.kaggle.com/c/titanic), [MNIST](https://www.kaggle.com/c/digit-recognizer) or [CIFAR-10](https://www.kaggle.com/pankrzysiu/cifar10-python). Even if these tasks are quite popular, it shall remain possible to add other tasks to the platform.

However, when I joined the project, the only way available to do so was to manually create a folder in the local file system of the server inside which a model and a weight file needed to be specified.

Furthermore, to make the changes on the deployed version, only the person who has the serval credentials can add the folder and redeploy the application.

All these steps make the process of adding a new task to the platform rather cumbersome and not scalable at all. A better alternative would be to make it crowdsourced. In other words, the task creation power is given back to the community through a form within the platform. This feature aims to tackle this issue.

### Implementation details

1. **Creation of the form template**: using the existing task configuration files (`.json`), I was able to build a template consisting of descriptive fields related to the task (e.g. description, features, learning rate, etc.). This template displays different fields according to the data type of the task (`CSV` or `Image`). Morevore, I implemented an automatic validation of input types according to the requirements of the platform.
1. **Creation of the Vue component**: the template in rendered inside the `NewTaskCreationForm` component which is accessible from the home page of the platform ([at the following link](https://epfml.github.io/DeAI/#/task-creation-form)).
1. **Form submission**: pushing the submission button triggers a `POST` request to the server which, in turn, creates the new task directory with the (`model.json `and `weights.bin` files input in the form) and updates the JSON configuration file of the tasks with the corresponding identifier and task description fields. When these elements are pushed to the server, all new users joining after the submission would automatically fetch the new task along with the others. However, the tricky part was to also make the changes available for the user who created the task. The solution was to use the global `store` of the platform so that the `TaskList` component was rerendered when a user presses the `send form` button inside the `NewTaskCreationForm` component.

> **Packages installed**: [vee-validate](https://vee-validate.logaretm.com/v4/), [axios](https://axios-http.com/), [yup](https://github.com/jquense/yup).

## Code Modularisation

### Motivation

### Implementation details

> **Packages installed**: `

## DeAI - FeAI merge

String resource
Internationalisation
Switching button

> **Packages installed**: [vue-i18n](https://vue-i18n.intlify.dev/)

## Logo for DeAI

The merge of `DeAI` and `FeAI` introduced the need for a new brand image for the project that conceals both platforms. A naming contest was launched for a few months which resulted in the adoption of the `DISCO` (distributed collaborative learning) name.

Incorporating both the design graphics of the website and the general idea of a disco ball.

After a few feedback sessions, the following logo ended up to be preferred by the members of the project :

XXXX

## Improve development experience

### Vue devtools

### Clean and update dependencies

### Add Documentation

## Separation Frontend / Backend code

## Command Line Interface (CLI)

### Barriers to the implementation

Issues :

- imports ES6 modules
- require construct is forbidden
- `peer-js` require browser environement (`window` and `navigator` objects) => `browser-env` package)
- No env environment dev/pred etc.
- WebRTC not possible with peerJS
- URL.createObjects
- Files are handled differently
- No FileReader and Files ⇒ file-api
- d3.csvParse ⇒ update d3 v6 to v7 for node module architecture

cleanest solution ⇒ global variables

- WebRTC not possible with peerJS => Question : `simple-peer` ?
- CSS color variable used in B informant use LocalStore which is not available in node js (code that myself)
- Better File loading (for multi class)
- Only CSV is currently supported

## Research for asynchronous training

# Conclusion

Necessary for further students : follow project guidelines (Vue and js) to avoid undoing the contribution.

1. Alternative to `peerjs`
1. Setup testing suite (JNEST)
1. Add support for new datatypes in task creation form
