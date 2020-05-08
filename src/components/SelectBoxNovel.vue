<template>
  <div class="hello">
    <h1>#金曜GUI</h1>
    <h2>📖プルダウン小説📖</h2>
    <div class="note">
      <h3>👍このGUIのポイント💡</h3>
      <ul>
        <li>進捗がわかりやすい</li>
        <li>ルビが出せない</li>
        <li>読みづらい</li>
      </ul>
    </div>

    <div class="ui">
      <label>
        １行あたりの長さ
        <input v-model.number="charsPerOption" type="number" min="1" />
      </label>

      <section class="progress">
        <p>{{ msg }}</p>
        <input v-model="progress" min="0" max="1" type="range" step="any" />
      </section>

      <select @change="onSelectChange" ref="select">
        <option v-for="(line, i) in lines" :key="i">{{ line }}</option>
      </select>
    </div>

    <div class="footnote">
      <a
        href="https://www.aozora.gr.jp/cards/000035/files/1567_14913.html"
        target="_blank"
        >走れメロス</a
      >
      <a
        href="https://twitter.com/Scott_Allen__/status/1258389642178514944"
        target="_blank"
        >インスパイア元</a
      >
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { text } from '@/assets/novel.ts'

// const charsPerOption = 20

export default Vue.extend({
  data() {
    return {
      charsPerOption: 20,
      selectedIndex: 0,
    }
  },
  computed: {
    lines(): string[] {
      return text.split('\n').reduce((acc: string[], line) => {
        if (line.length === 0) return [...acc, '']
        const strsInLine = [
          ...new Array(Math.ceil(line.length / this.charsPerOption)),
        ].map((_, i) => {
          const start = i * this.charsPerOption
          return line.substring(start, start + this.charsPerOption)
        })
        return [...acc, ...strsInLine]
      }, [])
    },
    progress: {
      get(): number {
        return (this.selectedIndex + 1) / this.lines.length
      },
      set(val: number) {
        const selectElement = this.$refs.select as HTMLSelectElement
        const index = Math.floor(val * (this.lines.length - 1))
        this.selectedIndex = selectElement.selectedIndex = index
      },
    },
    msg(): string {
      return `${(this.progress * 100).toFixed(2)}%読みました。  ${this
        .selectedIndex + 1} / ${this.lines.length}`
    },
  },
  methods: {
    onSelectChange(e: InputEvent) {
      this.selectedIndex = (e.target as HTMLSelectElement).selectedIndex
    },
  },
})
</script>

<style scoped lang="scss">
ul {
  list-style-type: none;
  padding: 0;
}
a {
  color: #42b983;
}
.note {
  background: #edd4f1;
  border-radius: 20px;
  padding: 4px;
  margin: 10px;
}

select {
  margin: 10px 0;
  font-size: 16px;
  border: rgb(120, 212, 83) 10px solid;
  padding: 10px;
}
.ui {
  margin: 100px 0;
}
.footnote {
  display: flex;
  flex-direction: column;
}
</style>
