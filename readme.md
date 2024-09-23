## setup

### frontend

1. open terminal
   2 create new project
   `mkdri example_project`
2. make frontend folder
   `cd example_project mkdir frontend`
3. go into new directory
   `cd frontend`
4. Crete new Vite project
   `npm create vite@latest frontend -- --template vue`
5. install dependencies
   `npm install`

### backend

1. Download Azure function Core Tools if necessary
2. cd into root example_project directory or open new terminal
3. make backend directory
   `mkdir backend`
4. cd into backend directory and generate base functions
   `func new --template "Http Trigger" --name "api" --worker-runtime typescript`
