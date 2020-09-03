<template>

  
   <div>
    <div class="header">   <b> Fancy Quiz App </b>     
   </div>
      <b-nav>
          <b-nav-item disabled> Counter: {{numCorrect}}/{{numTotal}} </b-nav-item>
     </b-nav>
   </div>
</template>

<script>
export default {
   props: [
      'numCorrect',
      'numTotal'
   ]
}
</script>
<style>
.header{
   font-family: perpetual;
   font-size: 60px;
   color: #255780;
}
</style>