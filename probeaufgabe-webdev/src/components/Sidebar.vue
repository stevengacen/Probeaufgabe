<template>
<div class="sidebar">
    <nav>
        <ul>
            <li>
                <router-link to="/" class="nav-item" active-class="active">
                    <GameboardIcon class="icon" />
                    <span>game board</span>
                </router-link>
            </li>
            <li>
                <router-link to="/user" class="nav-item" active-class="active">
                    <UserIcon class="icon" />
                    <span>user</span>
                </router-link>
            </li>
        </ul>
    </nav>
</div>
</template>

<script>
import GameboardIcon from '@/components/GameboardIcon.vue';
import UserIcon from '@/components/UserIcon.vue';

export default {
    name: 'Sidebar',
    components: {
        GameboardIcon,
        UserIcon
    }
};
</script>

<style>
.sidebar {
    width: 11vH;
    background-color: white;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px 0;
    -webkit-box-shadow: 6px 0px 8px -3px rgba(0, 0, 0, 0.08);
    -moz-box-shadow: 6px 0px 8px -3px rgba(0, 0, 0, 0.08);
    box-shadow: 6px 0px 8px -3px rgba(0, 0, 0, 0.08);
    z-index: 1001;
}

nav ul {
    list-style-type: none;
    padding: 0;
}

nav ul li {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 20px 0;
    color: #55555A;
    font-size: 13px;
    cursor: pointer;
}

nav ul li .nav-item {
    display: flex;
    flex-direction: column;
    align-items: center;

}

nav ul li .icon {
    width: 5vh;
    /* Adjust size as necessary */
    height: 5vh;
    /* Adjust size as necessary */
    background-color: white;
    border: 1px solid #55555A;
    border-radius: 5px;
    padding: 5px;
    fill: #55555A;
    -webkit-box-shadow: 0px 0px 12px 0px rgba(0, 0, 0, 0.33);
    -moz-box-shadow: 0px 0px 12px 0px rgba(0, 0, 0, 0.33);
    box-shadow: 0px 0px 12px 0px rgba(0, 0, 0, 0.33);
    transition: 0.1s all;

}

.nav-item span {
    margin-top: 10px;
    margin-bottom: 20px;
    text-align: center;
    color: #55555A;
    font-weight: 500;
    text-decoration: none;
}

.sidebar a {
    text-decoration: none;
}

.nav-item:hover .icon {
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
</style>
