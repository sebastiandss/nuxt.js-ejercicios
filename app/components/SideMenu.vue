<template>
  <aside class="side-menu">
    <div class="menu-content">
      <div class="menu-header">
        <img src="/images/icon-new-pb.svg" alt="" width="35px">
        <h2>Menu</h2>
      </div>
      
      <nav class="menu-links">
        <NuxtLink to="/" class="menu-item active">
          <span class="icon">speed</span> 
        </NuxtLink>
        <NuxtLink to="/inicio" class="menu-item">
          <span class="icon">mas</span> 
        </NuxtLink>
        <NuxtLink to="/proyectos" class="menu-item">
          <span class="icon">mas</span> 
        </NuxtLink>
      </nav>
    </div>
  </aside>
</template>

<style scoped>
    @import url('https://fonts.googleapis.com/css2?family=Sora:wght@400;600&display=swap');

    .side-menu {
        position: fixed;
        left: 0;
        top: 0;
        height: 100vh;
        width: 260px;
        /* Color camuflado con el fondo principal */
        background-color: rgb(15, 15, 15); 
        border-right: 1px solid rgb(15, 15, 15); 
        z-index: 1000;
        font-family: 'Sora', sans-serif;
        
        /* Estado inicial: escondido */
        transform: translateX(-250px); 
        transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    }

    /* Efecto al pasar el cursor por el borde izquierdo */
    .side-menu:hover {
        transform: translateX(0);
        box-shadow: 10px 0 30px rgba(0, 0, 0, 0.5);
    }

    .menu-content {
        padding: 3rem 1.5rem;
    }

    .menu-header {
        display: flex;
        align-items: center;
        gap: 12px;
        margin-bottom: 3rem;
    }

    .menu-header h2 {
        color: white;
        font-size: 1.2rem;
        margin: 0;
    }


    .menu-links {
        display: flex;
        flex-direction: column;
        gap: 0.8rem;
    }

    .menu-item {
        color: #64748b;
        text-decoration: none;
        font-size: 0.95rem;
        padding: 0.8rem 1.2rem;
        border-radius: 10px;
        transition: all 0.3s ease;
        display: flex;
        align-items: center;
        gap: 15px;
    }

    .menu-item:hover {
        background: rgba(255, 255, 255, 0.03);
        color: white;
    }

    .menu-item.active {
        background: rgba(59, 130, 246, 0.1);
        color: #3b82f6;
        font-weight: 600;
    }

    .icon {
        font-size: 1.1rem;
    }
</style>