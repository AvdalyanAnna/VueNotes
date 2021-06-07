<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <!-- message -->
          <message v-if="message" :message="message"/>

          <!-- new note -->
          <new-note
              :note="note"
              :types="types"
              @addNote="addNote"/>


          <div class="note-header">
            <!-- title -->
            <h2> {{ title }} </h2>
            <div class="d-flex">
              <!-- search -->
              <search
                  :value="search"
                  placeholder="Find your Note"
                  @search=" search = $event "/>

              <!-- type -->
              <filter-type
                  :types="types"
                  @changeType="changeType"/>
            </div>


            <!-- icon content -->
            <div class="icons">
              <svg :class="{ active:grid }" @click="grid = true"
                   xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none"
                   stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect x="3" y="3" width="7" height="7"></rect>
                <rect x="14" y="3" width="7" height="7"></rect>
                <rect x="14" y="14" width="7" height="7"></rect>
                <rect x="3" y="14" width="7" height="7"></rect>
              </svg>
              <svg :class="{ active: !grid }" @click="grid = false"
                   xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none"
                   stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <line x1="8" y1="6" x2="21" y2="6"></line>
                <line x1="8" y1="12" x2="21" y2="12"></line>
                <line x1="8" y1="18" x2="21" y2="18"></line>
                <line x1="3" y1="6" x2="3" y2="6"></line>
                <line x1="3" y1="12" x2="3" y2="12"></line>
                <line x1="3" y1="18" x2="3" y2="18"></line>
              </svg>
            </div>
          </div>

          <!-- note list -->
          <notes
              :notes="notesFilter"
              :grid="grid"
              @showInput="showInput"
              @changeTitle="changeTitle"
              @oldTitle="titleOld"
              @remove="removeNote"/>

        </div>
      </section>
    </div>
  </div>
</template>

<script>
import Message from "@/components/Message";
import NewNote from "@/components/NewNote";
import Notes from "@/components/Notes";
import Search from "@/components/Search";
import FilterType from "@/components/FilterType";

export default {
  name: 'App',
  data() {
    return {
      title: 'Notes App',
      oldTitle: null,
      selectValue: "all",
      search: '',
      message: null,
      grid: true,
      types: ['Low', 'Medium', 'High'],
      note: {
        title: '',
        description: "",
        type: 0,
      },
      notes: [
        {
          title: "First Note",
          description: "Description for first note",
          type: "Low",
          changeTitle: false,
          date: new Date(Date.now()).toLocaleString(),
        },
        {
          title: "Second Note",
          description: "Description for second note",
          type: "Medium",
          changeTitle: false,
          date: new Date(Date.now()).toLocaleString()

        },
        {
          title: "Third Note",
          description: "Description for third note",
          type: "High",
          changeTitle: false,
          date: new Date(Date.now()).toLocaleString()
        }
      ]
    }
  },
  methods: {
    addNote() {
      let {title, description, type} = this.note;
      if (title === '') {
        this.message = "Title can't be blank!";
        return false;
      }
      if (type === 0) {
        this.message = "Type not selected!";
        return false;
      }
      if (description === '') {
        this.message = "Description can't be blank!";
        return false;
      }
      this.notes.push({
        title,
        description,
        type,
        changeTitle: false,
        date: new Date(Date.now()).toLocaleDateString()
      })
      this.message = null;
      this.note.title = '';
      this.note.description = '';
      this.note.type = 0;
    },

    removeNote(index) {
      this.notes.splice(index, 1);
    },

    showInput(index) {
      for (let i = 0; i < this.notes.length; i++) {
        this.notes[i].changeTitle = false;
      }
      this.notes[index].changeTitle = true;
      this.oldTitle = this.notes[index].title;
    },

    changeTitle(index) {
      if (this.notes[index].title === '') {
        alert("Title can't be blank!");
        return false;
      }
      this.notes[index].date= new Date(Date.now()).toLocaleDateString()
      this.notes[index].changeTitle = false;
      this.oldTitle = null;
    },

    titleOld(index) {
      this.notes[index].changeTitle = false;
      this.notes[index].title = this.oldTitle;
      this.oldTitle = null;
    },

    changeType(selectValue) {
      this.selectValue = selectValue.toLowerCase()
    }
  },
  computed: {
    notesFilter() {
      let array = this.notes,
          search = this.search,
          type = this.selectValue;

      if (type != 'all') {
        array = array.filter(function (item) {
          if (search) {
            // Small
            search = search.trim().toLowerCase();
            //Filter
            if (item.type.toLowerCase().indexOf(type) !== -1 && item.title.toLowerCase().indexOf(search) !== -1) {
              return item
            }
          } else {
            //Filter
            if (item.type.toLowerCase().indexOf(type) !== -1) {
              return item
            }
          }
        })
      } else {
        if (!search) return array;
        // Small
        search = search.trim().toLowerCase();
        //Filter
        array = array.filter(function (item) {
          if (item.title.toLowerCase().indexOf(search) !== -1) {
            return item
          }
        })
      }

      // Error
      return array

    },

  },
  components: {
    Message,
    NewNote,
    Notes,
    Search,
    FilterType
  }
}
</script>

<style lang="scss" scoped>
.d-flex {
  display: flex;
  justify-content: space-between;
}
</style>
