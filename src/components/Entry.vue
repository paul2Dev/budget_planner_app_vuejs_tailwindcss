<template>
<li>
  <div class="content-entry">
      <div :class="(entry.type.title === 'Income') ? 'badge-green-sm' : 'badge-red-sm'">
          {{entry.type.title}}
      </div>
      <div class="badge-indigo-sm">
          {{entry.category.title}}
      </div>
      <div class="badge-dark-sm">
          <p>
              {{entry.title}}
          </p>
          <!-- <p>
              {{entry.description}}
          </p> --> 
      </div>
      <div :class="(entry.type.title === 'Income') ? 'badge-green-sm' : 'badge-red-sm'">
          {{ entry.type.title === 'Income' ? '+' : '-' }}  {{ parseInt(entry.value).toLocaleString("en-US") }} {{ currency }}
      </div>
      <div class="badge-dark-sm">
          {{entry.created_at}}
      </div>
      <div class="actions">
        <router-link :to="'/entries/' + entry.id" class="edit-icon">
          <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M13.586 3.586a2 2 0 112.828 2.828l-.793.793-2.828-2.828.793-.793zM11.379 5.793L3 14.172V17h2.828l8.38-8.379-2.83-2.828z"></path></svg>
        </router-link>
        <span @click="deleteEntry(entry.id)" class="delete-icon">
          <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd"></path></svg>
        </span>
      </div>
  </div>
</li>
</template>

<script>
import DataService from '@/services/DataService'
export default {
  name: 'Entry',
  props: {
    entry: {
      type: Object,
      required: true
    },
    currency: {
      type: String,
      required: true
    }
  },
  methods: {
    deleteEntry(id) {
      if(confirm('delete ' + '"'+ this.entry.title +'" ?')){
        DataService.deleteItem('entries', id)
        .then(response => {
          this.$store.dispatch('fetchBudgetsWithMonthlyEntries')
        })
        .catch(error => {
          console.log(error)
        })
        
      }
    }
  },
}
</script>
