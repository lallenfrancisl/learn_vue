Vue.component('cat-list', {
  props: ['cats'],
  template: `
    <ul>
      <li v-for="cat in cats">{{cat}}</li>
    </ul>

  `
})

new Vue({
    el: '#root',
    data: {
        greeting: 'Hello Vue!',
        count: 1,
        email: '',
        cats: [
          'Allen',
          'gifty',
          'AR',
          'Valikkandi',
          'kurippu',
          'kala'
        ],
        newCat: ''
    },
    methods: {
      addCat: function() {
        this.cats.push(this.newCat);
        this.newCat = '';
      }
    },
    filters: {
      capitalize: (val) => {
        return val.toUpperCase();
      }
    }
})
