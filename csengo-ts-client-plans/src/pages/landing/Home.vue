<template>
  <div class="main-container">
    <div class="header">
      <div>
        <div class="title">
          <span class="pollak">POLLAK</span>
          <span class="csengo">CSENGŐ</span>
        </div>
        <div class="welcome">ÜDVÖZLÜNK, valaki</div>
      </div>
      <div class="user-icon">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white" width="48px" height="48px">
          <path d="M12 2C9.79 2 8 3.79 8 6s1.79 4 4 4 4-1.79 4-4-1.79-4-4-4zm0 10c-4.41 0-8 2.24-8 5v3h16v-3c0-2.76-3.59-5-8-5z"/>
        </svg>
      </div>
    </div>
    <div class="container">
      <div class="card-body">
        <p>Card Body 1</p>
      </div>
      <div class="card-body">
        <p>Card Body 2</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardLayout",
};
</script>

<style scoped>
.main-container {
  position: relative;
  padding-top: 120px; 
  padding-left: 15px; 
  padding-right: 15px; 
}

.header {
  position: absolute;
  top: 10px;
  left: 10px;
  right: 30px;
  display: flex;
  justify-content: space-between; 
  align-items: flex-start; 
  font-family: 'Anta';
  color: white;
  padding-left: 20px;
  padding-top: 20px;  
}

.title {
  display: flex;
  gap: 5px;
}

.pollak {
  font-size: 2rem; 
  font-weight: bold;
}

.csengo {
  font-size: 2rem;
  font-weight: bold;
  color: red; 
}

.welcome {
  font-size: 1.2rem; 
  margin-top: 5px; 
  text-align: center; 
  padding-left: 20px; 
  padding-right: 20px; 
}

.user-icon {
  width: 48px; 
  height: 48px;
}

.card-body {
  font-family: 'Anta';
  position: relative;
  width: 90%;
  max-width: 600px; 
  min-width: 600px;
  height: 700px;
  padding: 20px;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(50px);
  box-sizing: border-box;
  box-shadow: 0 15px 25px rgba(0, 0, 0, 0.6);
  border-radius: 10px;
  text-align: center;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  word-wrap: break-word;
}

.card-body p {
  font-size: 1.2rem; 
  margin: 0;
}

.container {
  display: flex;
  justify-content: space-evenly; 
  align-items: center; 
  min-height: 80vh;
  padding: 20px; 
  gap: 50px; 
}

@media (max-width: 1300px) {
  .pollak, .csengo {
    font-size: 1.5rem; 
  }

  .welcome {
    font-size: 1rem;
  }

  .container {
    flex-direction: column;
    gap: 20px;
  }

  .card-body {
    min-width: 100%; 
    max-width: none;
  }

  .card-body p {
    font-size: 1rem; 
  }

  .user-icon {
    width: 32px; 
    height: 32px;
  }
}

@media (max-width: 750px) {
  .card-body {
    min-width: 110%; 
    max-width: none;
  }
}

@media (max-width: 350px) {
  .header {
    right: 30px;
  }

  .user-icon {
    width: 25px; 
    height: 25px;
  }

  .card-body {    
    min-width: 125%;
    padding-top: 180px;
  }
}
</style>
