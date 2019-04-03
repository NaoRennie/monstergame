<template>
  <div>
    <div class="ui segment">
      <div class="ui two column very relaxed grid">
        <div class="column">
          <p>you</p>
          <p class="bar"></p>
        </div>
        <div class="column">
          <p>monster</p>
          <p class="bar"></p>
        </div>
      </div>
      <div class="ui vertical divider">vs</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App"
};
</script>

<style>
.bar {
  display: inline-block;
  background-color: pink;
  width: 100px;
  height: 10px;
}
</style>
