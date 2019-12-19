<template>
  <div class="home">
    <section>
      <div class="title">
        <h1>Editor</h1>
      </div>
      <div class="row">
        <div class="editor">
          <textarea class="text" ref="textArea" :value="text"></textarea>
        </div>
        <div class="results">
          <pre></pre>
        </div>
      </div>
      <div class="row">
        <div class="btn-container">
          <button>Compilar y ejecutar</button>
        </div>
      </div>
    </section>
  </div>
</template>

<script lang="ts">
import HelloWorld from "@/components/HelloWorld.vue";
import { reactive, ref, onMounted, Ref } from "@vue/composition-api";
import CodeMirror from "codemirror";
import { Editor, EditorChange } from "codemirror";
import "codemirror/mode/clike/clike";
import "codemirror/lib/codemirror.css";
import "codemirror/theme/material.css";

export default {
  name: "home",
  setup() {
    const textArea: Ref<HTMLTextAreaElement | null> = ref(null);
    const cm: Ref<Editor | null> = ref(null);
    const text = ref("Buen día!");

    onMounted(() => {
      if (textArea.value) {
        cm.value = CodeMirror.fromTextArea(textArea.value, {
          mode: "text/x-csrc",
          theme: "material",
          lineNumbers: true
        });

        cm.value.on("change", change);
      }
    });

    function change(instance: Editor, changeObj: EditorChange) {
      text.value = instance.getValue();
    }

    return {
      text,
      textArea,
      cm,
      change
    };
  }
};
</script>

<style lang="scss" scoped>
.home {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.row {
  display: flex;
  width: 80vw;
}

.title {
  text-align: left;
}

.editor {
  text-align: left;
  margin-right: 5px;
  width: 50%;
}
.results {
  box-sizing: border-box;
  flex-grow: 1;
  background-color: #263238;
  margin-left: 5px;
  color: #fff;
  text-align: left;
  border-radius: 5px;
  height: 300px;
  padding: 0 10px;
  overflow-y: auto;

  pre {
    font-family: "Inconsolata", monospace;
    font-size: 16px;
    white-space: pre-wrap;
    padding: 4px 0;
    margin: 0;
  }
}

.btn-container {
  display: flex;
  justify-content: center;
  margin: 1em 0;

  button {
    font-family: "Inconsolata", monospace;
    font-size: 16px;
    color: #263238;
    background-color: #fff;
    float: left;
    padding: 0.6em 1.2em;
    text-decoration: none;
    border-color: #263238ce;
    border-radius: 5px;
    transition: all 0.2s;
  }

  button:hover {
    cursor: pointer;
    background-color: #263238;
    color: #fff;
  }
}
</style>
<style lang="scss">
.CodeMirror {
  font-family: "Inconsolata", monospace;
  font-size: 16px;
  border-radius: 5px;
}

.cm-s-material .CodeMirror-guttermarker,
.cm-s-material .CodeMirror-guttermarker-subtle,
.cm-s-material .cm-comment,
.cm-s-material .CodeMirror-linenumber {
  color: rgb(123, 159, 175);
}
</style>
