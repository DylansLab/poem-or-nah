<template>
  <div class="modal-backdrop" @click.self="close">
    <div class="modal">
      <section class="modal-body">
        <button type="button" class="close" @click="close">x</button>
        <h2 class="about-text">
          We've asked AI chat models to write us new poems based on the work of recognized poets.
          There are two real poems and one written by AI, can you tell which one it is?
        </h2>
        <h4>
          Too Easy? Hide authors to increase the difficulty.
        </h4>
        <br />
        Shoutout to <a href="https://poetrydb.org/index.html">poetrydb</a> and
        <a href="https://twitter.com/thundercomb">@thundercomb</a> for sourcing our real poems.
        <br />
        AI poems sourced from OpenAI. (2023). ChatGPT (Mar 14 version) and Bard, Google AI
      </section>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AboutModal',
  methods: {
    close() {
      this.$emit('close')
    }
  }
}
</script>

<style>
.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: lightsteelblue;
  color: black;
  box-shadow: 2px 2px 20px 1px;
  overflow-x: hidden;
  display: flex;
  flex-direction: column;
  max-width: 80vw;
  max-height: 75vh;
  border-radius: 15px;
  border: 5px solid lightslategray;
  min-height: 10vh;
  text-align: center;
}

.modal-body {
  position: relative;
  padding: 20px 10px;
}

.about-text {
  width: 80%;
  padding: 20px;
  margin: auto;
}

.close {
  position: absolute;
  top: -10px;
  right: -5px;
  border: none;
  font-size: 20px;
  padding: 10px;
  cursor: pointer;
  font-weight: bold;
  color: lightslategray;
  background: transparent;
}
</style>
