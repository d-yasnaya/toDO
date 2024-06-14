
<!DOCTYPE html>
<html lang="en">
<head>
    <link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TO DO LIST</title>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,700;1,700&display=swap" rel="stylesheet">
    <link rel="stylesheet" type="text/css" href="https://necolas.github.io/normalize.css/8.0.1/normalize.css">
    <link rel="stylesheet" href="/на сдачу/style5.css">
</head>
<body>
    <h1>To do list</h1>
    <div class="flex-wrapper">
        <div class="add-todo-wrapper">
            <div class="input-wrapper">
                <input onkeydown="return checkPhoneKey(event.key)" placeholder="Привет, чем мы сегодня займемся?" type="text" id="description-task">
            </div>
            <div class="button-wrapper">
                <button id="add-task-btn">Добавить</button>
            </div>
        </div>
        <h2>Выполнено:</h2>
        <div class="todos-wrapper">
        </div> 
    </div>
    <script src="/на сдачу/script5.js"></script>
</body>
</html>
html, body {
    margin: 0;
    height: 100%;
    background-color: #000000;
    font-family: "Montserrat", sans-serif;
}

h1 {
    text-align: center;
    color:#ffffff;
}

h2{
    text-align: center;
    color:#ffffff   
}

.flex-wrapper {
    display: flex;
    flex-flow: column;
    justify-content: center;
    align-items: center;
    width: 100%;
}

.add-todo-wrapper {
    display: flex;
    width: 600px;
    min-height: 40px;
    align-items: center;
    background-color: #262626;
    border-radius: 10px;
    padding: 10px;
}

.input-wrapper {
    width: 80%;
}

.input-wrapper input {
    width: 100%;
    height: 25px;
}

.button-wrapper {
    width: 20%;
    display: flex;
    justify-content: flex-end;
}

.button-wrapper button {
    width: 90%;
    background-color: #32742d;
    color: solid #fff;
    outline: none;
    border: none;
    height: 35px;
    cursor: pointer;
    border-radius: 10px;
}

.todo-item {
    display: flex;
    width: 600px;
    min-height: 50px;
    border: 1px solid #ffffff;
    border-radius: 10px;
    margin-bottom: 15px;
    padding: 10px;
    background-color: #32742d;
}

.todo-item.checked {
    background-color: #262626;
    opacity: 0.6;
    color: #ffffff
}


.todo-item.delition {
    animation: opacity 1s ease-in-out;
     border-radius: 10px;
}

.description {
    width: 80%;
    display: flex;
    align-items: center;
    color: #ffffff;
}

input::placeholder {
    color: #32742d;   
}  
  
input[type=text] {
    width: 400px;
    height: 25px;
    padding: 10px; 
    border-radius: 10px;
    border-color:#b9b8b8;
    color: #000000;
    font-weight: 700;
    transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
    background-color: #ffffff;

}

.buttons {
    width: 20%;
    display: flex;
    align-items: center;
    justify-content: flex-end;
}

.buttons input {
    width: 25px;
    height: 25px;
    cursor: pointer;
}

.buttons button {
    outline: none;
    border: none;
    background-color: #32742d;
    color: #ffffff;
    cursor: pointer;
    height: 25px;
    border-radius: 5px;
    opacity: 0.5;
}

@keyframes opacity {
    from {
        opacity: 1;
    }
    to {
        opacity: 0;
    }
}
const addTaskBtn = document.getElementById('add-task-btn'); // добавить
const deskTaskImput = document.getElementById('description-task'); // импут
const todoWrapper = document.querySelector('.todos-wrapper'); // запись задачи

let tasks;
!localStorage.tasks ? tasks = [] : tasks = JSON.parse(localStorage.getItem('tasks'));

let todoItemeElems = [];

function Task(description) { // шаблон задачи
    this.description = description; 
    this.completed = false; // статус задачи
}

const createTemlate = (task, index) => {
    return `
    <div class='todo-item ${task.completed ? 'checked' : ''}'>
        <div class='description'>${task.description}</div>
        <div class='buttons'>
          <input onclick="completeTask(${index})" class='btn-comlete' type="checkbox" ${task.completed ? 'checked' : ''}>
          <button onclick="deleteTask(${index})" class='btn-delete'><i class="material-icons">delete_outline </i></button>
        </div>
    </div>
    `
}

const filterTasks = () => {
    const activeTasks = tasks.length && tasks.filter(item => item.completed == false);
    const completedTasks = tasks.length && tasks.filter(item => item.completed == true);
    tasks = [...activeTasks,...completedTasks];
}

const fillHtmlList = () => {
    todoWrapper.innerHTML = '';
    if (tasks.length > 0) {
        filterTasks();
        tasks.forEach((item, index) => {
            todoWrapper.innerHTML += createTemlate(item, index);
        })
        todoItemeElems = document.querySelectorAll('.todo-item');
    }
};

fillHtmlList();

const updateLocal = () => {
    localStorage.setItem('tasks', JSON.stringify(tasks));
};

const completeTask = index => {
    tasks[index].completed = !tasks[index].completed;
    if (tasks[index].completed) {
        todoItemeElems[index].classList.add('checked');
    } else {
        todoItemeElems[index].classList.remove('checked');
    }
    updateLocal(); 
    fillHtmlList();
};


addTaskBtn.addEventListener('click', () => { // работа кнопки добавить
    tasks.push(new Task(deskTaskImput.value)); // перевод в конец
    updateLocal();
    fillHtmlList();
    deskTaskImput.value = '';
});

function checkPhoneKey(key) {
    if (key == 'Enter') {
        tasks.push(new Task(deskTaskImput.value)); // 
        updateLocal();
        fillHtmlList();
        deskTaskImput.value = '';
        };
};


const deleteTask = index => {
    tasks.splice(index, 1);
    updateLocal();
    fillHtmlList();
}
