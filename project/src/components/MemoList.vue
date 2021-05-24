<template>
  <div>
    <button class="new_button" @click="resetView">New</button>
    <div class="memolist_container">
      <div class="edit_memo">
        <div v-if="editMode">
            <h3>Edit</h3>
            <p class="memo">{{ title(editMemoValue) }}</p>
            <textarea v-model="editMemoValue"/>
            <div>
              <button @click="updateMemo()">Update</button>
              <button @click="removeMemo()">Remove</button>
            </div>
        </div>
        <div v-else>
            <h3>New</h3>
            <textarea v-model="newMemo"/>
            <div>
              <button @click="addMemo">Add Memo</button>
            </div>
        </div>  
      </div>
      <div class="list_memo">
        <ol 
          v-for="(value, key) in memos"
          v-bind:key="key"
          v-bind:value="value"
        >
          <li class="list_items">
            <a href="#" class="list_item__title" @click="editMemo(key)">{{ title(value) }}</a>
          </li>
        </ol>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'MemoList',
  data() {
    return {
      memos: [],
      newMemo: null,
      editMemoKey: null,
      editMemoValue: null,
      editMode: false
    }
  },
  mounted() {
    if (localStorage.getItem('memos')) {
      try {
        this.memos = JSON.parse(localStorage.getItem('memos'))
      } catch(e) {
        localStorage.removeItem('memos')
      }
    }
  },
  methods: {
    addMemo() {
      if (!this.newMemo) {
        return
      }
      this.memos.push(this.newMemo)
      this.newMemo = null
      this.saveMemos()
    },
    editMemo(key) {
      this.editMode = true
      this.editMemoKey = key
      this.editMemoValue = this.memos[key]
    },
    updateMemo() {
      if (!this.editMemoValue) {
        return
      }
      this.memos.splice(this.editMemoKey, 1, this.editMemoValue)
      this.editMemoValue = null
      this.saveMemos()
    },
    removeMemo() {
      this.memos.splice(this.editMemoKey, 1)
      this.editMemoValue = null
      this.saveMemos()
    },
    saveMemos() {
      const parsed = JSON.stringify(this.memos)
      localStorage.setItem('memos', parsed)
    },
    title(content) {
      return content.split('\n')[0]
    },
    resetView() {
      this.editMode = false
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.memolist_container {
  display: flex;
}
.new_button {
  margin-bottom: 1em;
}
.list_memo {
  padding: 1em;
}
.edit_memo {
  padding: 1em;
  border: 1px solid black;
  display: flex;
}
.list_items {
  display: flex;
}
textarea {
  width: 200px;
  height: 320px;
}
</style>
