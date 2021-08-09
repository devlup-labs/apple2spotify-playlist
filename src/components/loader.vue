<template>
  <div class="backdrop">
    <div class="modal flex justify-center" v-if="render">
      <div class="loader">
        <bar />
          <div class="status text-center">
            <h1>{{ text }}</h1>
          </div>
      </div>
    </div>
  </div>
</template>

<script>
import bar from "./bar";

export default {
  name: "loader",
  props: ["text", "render"],
  components: { bar },
};
</script>

<style>
.modal {
  width: 36%;
  padding: 20px;
  padding-top: 40px;
  margin: auto;
  margin-top: 200px;
  background: #292b2c;
  border-radius: 10px;
}
.backdrop {
  top: 0;
  position: fixed;
  background: rgba(0, 0, 0, 0.5);
  padding-right: 100px;
  width: 100%;
  height: 100%;
}
.status {
  color: whitesmoke;
  padding-top: 20px;
  font-style: normal;
  font-size: larger;
}

.loader {
  border-radius: 10px;
}
</style>
