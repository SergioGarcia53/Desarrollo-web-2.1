<template>
    <v-data-table
      :headers="headers"
      :items="formattedPosts"
      :sort-by="[{ key: 'id', order: 'asc' }]"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Mi tabla CRUD</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ props }">
              <v-btn class="mb-2" color="primary" dark v-bind="props">Nuevo post</v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="text-h5">{{ formTitle }}</span>
              </v-card-title>
              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12">
                      <v-text-field v-model="editedItem.authorName" label="Author"></v-text-field>
                    </v-col>
                    <v-col cols="12">
                      <v-text-field v-model="editedItem.title" label="Title"></v-text-field>
                    </v-col>
                    <v-col cols="12">
                      <v-text-field v-model="editedItem.body" label="Post"></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" variant="text" @click="close">Cancelar</v-btn>
                <v-btn color="blue darken-1" variant="text" @click="save">Guardar</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="text-h5">Esta seguro que lo quiere eliminar?</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" variant="text" @click="closeDelete">Cancelar</v-btn>
                <v-btn color="blue darken-1" variant="text" @click="deleteItemConfirm">OK</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item.actions="{ item }">
        <v-icon class="me-2" size="small" @click="editItem(item)">mdi-pencil</v-icon>
        <v-icon size="small" @click="deleteItem(item)">mdi-delete</v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize">Reset</v-btn>
      </template>
    </v-data-table>
  </template>
  
  <script>
  export default {
    data: () => ({
      dialog: false,
      dialogDelete: false,
      headers: [
        { title: 'Author', key: 'authorName', align: 'start' },
        { title: 'Title', key: 'title' },
        { title: 'Post', key: 'body' },
        { title: 'Actions', key: 'actions', sortable: false },
      ],
      posts: [],
      editedIndex: -1,
      editedItem: {
        authorName: '',
        title: '',
        body: '',
      },
      defaultItem: {
        authorName: '',
        title: '',
        body: '',
      },
    }),
  
    computed: {
      formTitle() {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item';
      },
      formattedPosts() {
        return this.posts;
      }
    },
  
    watch: {
      dialog(val) {
        val || this.close();
      },
      dialogDelete(val) {
        val || this.closeDelete();
      },
    },
  
    created() {
      this.initialize();
    },
  
    methods: {
      async initialize() {
        try {
          const [postsResponse, usersResponse] = await Promise.all([
            fetch('https://jsonplaceholder.typicode.com/posts').then(response => response.json()),
            fetch('https://jsonplaceholder.typicode.com/users').then(response => response.json())
          ]);
          this.posts = postsResponse.map(post => ({
            ...post,
            authorName: this.getUserNameById(usersResponse, post.userId),
          }));
        } catch (error) {
          console.error('Error fetching data:', error);
        }
      },
  
      editItem(item) {
        this.editedIndex = this.posts.indexOf(item);
        this.editedItem = { ...item };
        this.dialog = true;
      },
  
      deleteItem(item) {
        this.editedIndex = this.posts.indexOf(item);
        this.editedItem = { ...item };
        this.dialogDelete = true;
      },
  
      deleteItemConfirm() {
        this.posts.splice(this.editedIndex, 1);
        this.closeDelete();
      },
  
      close() {
        this.dialog = false;
        this.$nextTick(() => {
          this.editedItem = { ...this.defaultItem };
          this.editedIndex = -1;
        });
      },
  
      closeDelete() {
        this.dialogDelete = false;
        this.$nextTick(() => {
          this.editedItem = { ...this.defaultItem };
          this.editedIndex = -1;
        });
      },
  
      save() {
        if (this.editedIndex > -1) {
          this.posts.splice(this.editedIndex, 1, { ...this.editedItem });
        } else {
          this.posts.push({ ...this.editedItem });
        }
        this.close();
      },
  
      getUserNameById(users, userId) {
        const user = users.find(user => user.id === userId);
        return user ? user.name : '';
      },
    },
  };
  </script>
  