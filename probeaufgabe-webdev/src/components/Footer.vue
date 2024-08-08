<template>
<div class="footer">
    <img src="@/assets/logo-flexlog.svg" alt="Flexlog Logo" class="logo">
    <span class="footerspan"> test exercise</span>
    <span class="footerspan"> 06.08.24</span>
    <span class="footerspan"> Steven Gacenbiler</span>
</div>
</template>

  
<script>
export default {
    name: 'Footer'
};
</script>

  
<style>
.footer {
    display: flex;
    flex-direction: row;
    align-items: center;
    width: 100%;
    height: 20px;
    margin-left: 100px;
    position: fixed;
    bottom: 0;
    background-color: rgba(255, 255, 255, 0.3);
    color: black;
    font-size: 10px;
    text-align: left;
    padding: 2px 2px;
    vertical-align: middle;
}

.footerspan {
    margin-left: 20px;
    color: #d8d8d8;
    font-weight: 600;
}

.logo {
    height: 10px;
    margin-left: 20px;
}
</style>
