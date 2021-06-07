<template>
  <!--    note list -->
  <div class="notes">
    <div class="note"
         :class="{full : !grid}" v-for="(note,index) in notes"
         :key="index">

      <div class="note-header" :class="{full : !grid}">
        <input
            v-if="note.changeTitle"
            type="text"
            v-model="note.title"
            @keyup.enter="changeTitle(index)"
            @blur="changeTitle(index)"
            @keyup.esc="oldTitle(index)"
        >
        <p v-else @click="showInput(index)">{{ note.title }}</p>
        <p @click="removeNote(index)">x</p>
      </div>

      <div class="note-body">
        <p>{{ note.description }}</p>
        <div class="d-flex">
          <span>{{ note.date }}</span>
          <span class="type" :class="note.type.toLowerCase()">{{ note.type }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Notes",
  props: {
    notes: {
      type: Array,
      required: true,
    },

    grid: {
      type: Boolean,
    }
  },
  methods: {
    removeNote(index) {
      this.$emit('remove', index)
    },

    showInput(index){
      this.$emit('showInput', index)
    },

    changeTitle(index){
      this.$emit('changeTitle', index)
    },

    oldTitle(index){
      this.$emit('oldTitle', index)
    }
  }
}
</script>

<style lang="scss">
.notes {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  padding: 40px 0;
}

.note {
  width: 48%;
  padding: 18px 20px;
  margin-bottom: 20px;
  background-color: #fff;
  transition: all .25s cubic-bezier(.02, .01, .47, 1);
  box-shadow: 0 30px 30px rgba(0, 0, 0, 0.02);

  &:hover {
    box-shadow: 0 30px 30px rgba(0, 0, 0, 0.04);
    transform: translate(0, -6px);
    transition-delay: 0s !important;
  }

  &.full {
    width: 100%;
    text-align: center;
  }

}

.note-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  input{
    margin-bottom: 0;
    width: 80%;
    padding: 8px;
    font-size: 16px;
  }

  &.full {
    justify-content: center;

    p {
      margin-right: 16px;

      &:last-child {
        margin-right: 0;
      }
    }
  }

  p {
    color: #402caf;
    font-size: 22px;

    &:nth-last-child(1) {
      cursor: pointer;
    }
  }

  h2 {
    font-size: 32px;
  }

  svg {
    margin-right: 12px;
    color: #999;
    cursor: pointer;

    &.active {
      color: #402caf;
    }

    &:last-child {
      margin-right: 0;
    }
  }
}

.note-body {
  p {
    margin: 20px 0;
  }

  span {
    font-size: 14px;
    color: #999;
  }
}

.d-flex {
  display: flex;
  justify-content: space-between;
}

span.type {
  font-size: 12px;
  color: #fff !important;
  border-radius: 15px;
  max-width: 75px;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 5px 10px;

  &.low {
    background-color: #19db7e;
  }

  &.medium {
    background-color: #ff7511;
  }

  &.high {
    background-color: #fb5779;
  }

}
</style>