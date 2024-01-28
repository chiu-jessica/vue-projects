<template>
    <div id = "nav-bar">
        <nav>
            <a href = "/">HOME</a>
            <a href = "/quiz">QUIZ</a>
            <a href = "/password-gen">PASSWORD GEN</a>
            <a href = "/weather">WEATHER</a>
            <a href = "/agenda">AGENDA</a>
        </nav>
    </div>
</template>

<style>
    #nav-bar {
        padding: 15px 20px 15px;
        background-color: #7d4739;
        box-shadow: 0px 3px 10px rgba(0, 0, 0, 0.2);
        z-index: 1000;
        position: fixed;
        width: 100%;
    }
    a {
        font-family: 'Playfair Display', serif;
        text-decoration: none;
        color: white;
        font-size: 20px;
        margin-right: 30px;
    }
</style>

