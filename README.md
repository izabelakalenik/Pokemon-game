# Pokémon Route 1 Web Application

This project, developed as a part of a System and Web Technologies course - 6th semester, was done in a group of 2 people.
<br/>
<br/>

## Project Overview
The project is a simplified web-based version of Pokémon Red's Route 1 experience, developed using **Vue.js** for the *front-end* and **Node.js with Express.js** for the *back-end*.
<br/>
<br/>

## Front-End (Vue.js)
Built with **Vue.js**, this client allows players to explore the Route 1 map, encounter wild Pokémon, catch them, and track progress in a Pokedex.
The front-end communicates with the server using **HTTP requests** to simulate gameplay, including player movement, wild encounters, and saving game state.
* **Components:**
  <br/>
  Include middleware to manage interactions between base components (map, player, wild encounters, menu, and Pokedex) and back-end requests.
<br/>

## Back-End (Node.js)
The back-end, built with **Node.js and Express.js**, provides essential endpoints for game functionality:
* ***/initial_info:*** Returns initial game state or a saved state from a .txt file.
* ***/enter_grass & /leave_grass:*** Handles wild encounters.
* ***/capture:*** Handles Pokémon capture.
* ***/save:*** Stores game progress in a .txt file for future sessions.
<br/>

## Objective
The primary goal of this project is to integrate and apply core JavaScript concepts by developing a full-stack web application. It focuses on:

* Implementing a dynamic and interactive front-end using ***Vue.js***.
* Building a robust back-end with ***Node.js and Express.js*** to manage game state, player interactions, and Pokémon encounters.
* Enhancing understanding of client-server communication through ***RESTful API endpoints***.
* Simulating game mechanics from Pokémon Red in a ***simplified web environment***, offering players the ability to explore, catch Pokémon, and save progress.

