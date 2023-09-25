<script>
    import {onMount} from "svelte";
    import { Trash2 } from 'lucide-svelte';

    let inputVal = ""

    function button() {
        console.log("u press buton!1")
        let input = document.querySelector(".list-input")
        let value = input.value
        if (value == "") {
            alert("Put something")
            return
        }
       
        saveToStorage()
        items.todo.push(value)

        items = items;

    }
    function remove(e) {
        // e.target.removeEventlistener("click", remove)
        console.log(items);
        items.todo.splice(items.todo.indexOf(e.target.parentElement.innerText), items.todo.indexOf(e.target.parentElement.innerText) + 1)
        items.completed.push(e.target.parentElement.innerText)
        items = items;
        saveToStorage()
    }
    let items = {
        todo: [], 
        completed:[]
    }

    function loadFromStorage() {
        let data = localStorage.getItem("Items-todo") 
        if (data) {
            items = JSON.parse(data)
        }
    }


    function saveToStorage() {
        localStorage.setItem("Items-todo", JSON.stringify(items))
    }
    function removeall() {

        if (confirm("Are you sure you want to delete ALL items in this section?")){
            items.completed=[]
            items=items
            saveToStorage()
        }
    }
    onMount(() => {
        loadFromStorage()
    })

    $: if (inputVal.length > 40) {
        inputVal = inputVal.slice(0, 30)
    } 

</script>

<div class="box">
        <div class="square">
            <h1>To Do</h1>
            <ul class="list">
                {#each items.todo as todoValue}
                    <div>                  
                        <li>
                            <div on:click={remove} class="check"></div>{todoValue}
                        </li>
                    </div>
                {/each}
            </ul>
            {#if items.todo.length == 0}
            <h3>Nothing here yet!</h3>
        {/if}
            <div class="Trash-wrapper">
                <h1>Completed</h1>
                <button class="Trash-Button" on:click={removeall}><Trash2 /></button>
            </div >
            <ul class="list completed">
                <!-- DO EACH HERE -->
                {#each items.completed as completedValue}
                    <div><li>
                        <div class="check"></div>{completedValue}
                    </li></div>
                {/each}
            </ul>
            {#if items.completed.length == 0}
                <h3>Nothing here yet!</h3>
            {/if}
        </div>
        
    </div>

<div class="input-wrapper">
    <div class="input-container">
        <label for="tree">Cool Label</label>
        <input id="tree" placeholder="Homework, chores..." bind:value={inputVal} class="list-input">
    </div>
    <div class="input-container">
        <button type="button" class="Submit-button" on:click={button}>Add to List</button>
    </div>
</div> 
    
<style>
  .input-wrapper {
    display:flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height:60px;

  }


    * {
        font-family: 'Work Sans', sans-serif;
    }
    
    body {
        background-color: rgba((145, 120, 120, 0.284)251);
    }
    
    li {
        color: rgb(255, 255, 255);
        cursor: pointer;
        text-align: center;
        background-color: black;
        border-radius: 13px;
        margin-bottom: 13px;
        list-style: none;
        padding: 13px;
        align-items: center;
        max-width: 60% ;
        display: inline-flex;
    }
    
    .list-input {
        height: 40px;
        width: 60vw;
        border-radius: 15px;
        outline: none;
    }
    
    .list-input:focus {
        outline: none;
    }
    
    .box {
        width: 100vw;
        justify-content: center;
        align-items: center;
        gap: 100px;
        height:calc(100vh - 110px);
    }
    .check {
        background-color: #33335f;
        height: 15px;
        width: 15px;
        margin-right: 10px;
        border-radius: 50%;
    
    }
    
    .completed li .check {
        background-color: goldenrod !important;
    }
    
    .check:hover {
        background-color: goldenrod;
    }
    
    .Submit-button {
        background: #3b82f6;
        color: white;
        height: 40px;
        font-weight: 600;
        border: none;
        border-radius: 5px;
        padding-left: 10px;
        padding-right: 10px;
        transition: all;
        margin-top: 20px;
        margin-left: 10px;
    }
    
    .Submit-button:hover {
        background-color: #2563eb;
    }

    label {
        height: 10px;
    }
    .input-container {
        display: flex;
        flex-direction: column;
        gap: 10px;
    }
    .Trash-wrapper {
        display: flex;
        align-items: center;
    }
    .Trash-Button {
        all: unset;
        cursor:pointer;
        margin-left: 11px;
    }
</style>