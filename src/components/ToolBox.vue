<template>
  <div class="toolbox">
    <router-link to="/tools/xml-pretty">
      <div class="toolbox card">
        <div class="card-body">
          <div class="card-title p-3" style="
            background: #ff4f09;
            color: #fff;
            border-radius: 4px 30px 30px 30px;
          ">XML Pretty</div>
        </div>
      </div>
    </router-link>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
h2 {
  font-size: 1.2rem;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #f1592e;
}
.toolbox .card .card-body .card-title {
  height: max-content;
  white-space: pre-wrap;
  font-size: 14px;
  font-weight: bold;
  margin: auto;
}
.toolbox .card .card-body {
  display: flex;
  width: 100px;
  height: 100px;
  font-size: 14px;
  cursor: pointer;
}
.toolbox .card {
  transition-duration: .2s;
  transform: scale(.94);
  border-radius: 10px 30px;
}
.toolbox .card:hover {
  transform: scale(.98);
  border-radius: 10px 30px;
  border: 1px solid #ffa38a;
}
</style>
