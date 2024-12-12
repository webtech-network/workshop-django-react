# workshop-django-react

## Introdução

Bem-vindos ao workshop de React! Este repositório serve como apoio para acompanhar o conteúdo e realizar as práticas ao longo do curso. Qualquer dúvida, não hesite em recorrer aos instrutores que estarão disponíveis para ajudar.

## Criando o primeiro componente - Navbar

```bash
npm i react-router-dom
```

```jsx
import { Link } from "react-router-dom";
import style from "./Navbar.module.css";

function Navbar() {
  return (
    <header className={style.navbar}>
      <h1>
        <Link to="/">WebTech</Link>
      </h1>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/tasks/completed">Completed Tasks</Link> 
          </li>
        </ul>
      </nav>
    </header>
  );
}

export default Navbar;
```

```css
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 20px;
    background-color: inherit;
    margin-bottom: 50px;
  }
  
  .navbar h1 {
    font-size: 1.8rem;
  }
  
  .navbar a {
    color: black;
    font-weight: bold;
    text-decoration: none;
  }
  
  nav ul {
    list-style: none;
    margin: 0;
    padding: 0;
    display: flex;
  }
  
  nav li {
    margin-left: 20px;
  }
  
  nav a {
    font-size: 1rem;
    font-weight: 500;
    transition: color 0.3s ease;
  }
  
  nav a:hover {
    color: #61C9A8;
    text-decoration: underline;
  }
```

## Modificando a App
```
import React from 'react';
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import Navbar from './components/Navbar';
import Home from './pages/Home'; 
import CompletedTasks from './pages/CompletedTasks';  

function App() {
  return (
    <Router>
      <Navbar />
      <Routes>
        <Route path="/" element={<Home />} /> 
        <Route path="/tasks/completed" element={<CompletedTasks />} />  
      </Routes>
    </Router>
  );
}

export default App;
```

## Criando o Componente Tasks

```jsx
import React, { useEffect, useState } from 'react';
import style from './Tasks.module.css';

function Tasks({ showCompleted }) {
  const [tasks, setTasks] = useState([]);

  useEffect(() => {
    const loadData = async () => {
      try {
        const response = await fetch('http://localhost:8000/api/tasks');
        const data = await response.json();

        if (showCompleted) {
          setTasks(data.filter((task) => task.is_completed));
        } else {
          setTasks(data.filter((task) => !task.is_completed));
        }
      } catch (error) {
        console.error('Error fetching tasks:', error);
      }
    };
    loadData();
  }, [showCompleted]);

  return (
    <div className={style.TaskList}>
      <h2>{showCompleted ? 'Feitas' : 'To do'}</h2>
      {tasks.length === 0 ? (
        <p>No tasks to show!</p>
      ) : (
        tasks.map((task) => (
          <div key={task.id} className={style.TaskItem}>
            <h3>{task.name}</h3>
            <p>{task.description}</p>
            <p>Due: {task.due_to}</p>
          </div>
        ))
      )}
    </div>
  );
}

export default Tasks;
```

```css
.TaskList {
  padding: 20px;
  background-color: #61C9A8;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  max-width: 1000px;
  margin: 0 auto;
  font-family: Arial, sans-serif;
}

.TaskItem {
  background-color: #ffffff;
  padding: 15px;
  margin-bottom: 15px;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.CompleteButton {
  background-color: #4CAF50;
  color: white;
  border: none;
  padding: 10px 15px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
  margin: 10px 5px 0 0;
}

.CompleteButton:hover {
  background-color: #45a049;
}

.DeleteButton {
  background-color: #f44336;
  color: white;
  border: none;
  padding: 10px 15px;
  border-radius: 5px;
  cursor: pointer;
}

.DeleteButton:hover {
  background-color: #e53935;
}



@media (max-width: 768px) {
  .TaskList {
    padding: 15px;
    max-width: 100%;
  }
}
```

## Criando a Primeira página
```jsx
import React from 'react';
import Tasks from '../components/Tasks';

function Home() {
  return (
    <div>
      <Tasks showCompleted={false} /> 
    </div>
  );
}

export default Home;
```

## Criando a Segunda página
```jsx
import React from 'react';
import Tasks from '../components/Tasks'; 

function CompletedTasks() {
  return (
    <div>
      <Tasks showCompleted={true} /> 
    </div>
  );
}

export default CompletedTasks;
```

