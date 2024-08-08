<template>
    <div class="mobile-nav">
        <nav>
            <ul>
                <li>
                    <button class="mbl-nav-btn" @click="openMenu">|||</button>
                </li>
                <Transition name="slide-fade-mbl-nav">
                    <li v-if="isOpen">
                        <router-link to="/" class="nav-item" active-class="active">
                            <GameboardIcon @click="openMenu" class="icon"/>
                        </router-link>
                    </li>
                </Transition>
                <Transition name="slide-fade-mbl-nav2">
                    <li v-if="isOpen">
                        <router-link to="/user" class="nav-item" active-class="active">
                            <UserIcon @click="openMenu" class="icon"/>
                        </router-link>
                    </li>
                </Transition>
            </ul>
        </nav>
    </div>
</template>
  
<script>
  import GameboardIcon from '@/components/GameboardIcon.vue';
  import UserIcon from '@/components/UserIcon.vue';

  export default {
    name: 'MobileNav',
    components: {
      GameboardIcon,
      UserIcon
    },

    data() {
            return {
                isOpen: false,
            };
        },
        
        methods: {
            openMenu(isOpen) {
                
                this.isOpen = !this.isOpen;
            }
        }
  };
  
</script>

<style>
.mobile-nav {
    position: absolute;
    height: 100px;
    z-index: 100001;
}

.mbl-nav-btn {
    width: 5vh;
    height: 5vh;
    font-size: 20px;
    transform: rotate(-90deg);
    padding-bottom: 5px;
    padding-right: 5px;
}
    
nav ul {
    list-style-type: none;
    padding: 0;
}
    
nav ul li {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 10px 0px 0px 10px;
    color: #55555A;
    font-size: 13px;
    cursor: pointer;
}

  nav ul li .nav-item{
      display: flex;
      flex-direction: column;
      align-items: center;
      
  }

  nav ul li .icon {
      width: 4vh; /* Adjust size as necessary */
      height: 4vh; /* Adjust size as necessary */
      background-color: white;
      border: 1px solid #55555A;
      border-radius: 5px;
      padding: 5px;
      fill: #55555A;
      -webkit-box-shadow: 0px 0px 12px 0px rgba(0,0,0,0.33);
      -moz-box-shadow: 0px 0px 12px 0px rgba(0,0,0,0.33);
      box-shadow: 0px 0px 12px 0px rgba(0,0,0,0.33);
      transition: 0.1s all;
      
  }   

  .nav-item:hover .icon{
    background-color: #95C546;
    fill: white;
    border-color: #ffffff00;
    transition: 0.1s all;
  }

  .nav-item.active .icon {
    background-color: #95C546;
    fill: white;
    border-color: #ffffff00;
  }

  .slide-fade-mbl-nav-enter-active,
  .slide-fade-mbl-nav2-enter-active {
        transition: all 0.4s ease-out;
    }

    .slide-fade-mbl-nav-leave-active,
    .slide-fade-mbl-nav2-leave-active {
        transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
    }

    .slide-fade-mbl-nav-enter-from,
    .slide-fade-mbl-nav-leave-to {
        transform: translateY(-20px);
        opacity: 0;
    }

    .slide-fade-mbl-nav2-enter-from,
    .slide-fade-mbl-nav2-leave-to {
        transform: translateY(-60px);
        opacity: 0;
    }
</style>
  