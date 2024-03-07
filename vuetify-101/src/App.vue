<template>
  <v-app>
    <v-layout>
      <v-app-bar>
        <template v-slot:prepend>
          <v-app-bar-nav-icon @click.stop="drawerOpen = !drawerOpen"></v-app-bar-nav-icon>
        </template>
        <v-app-bar-title>MENU</v-app-bar-title>
      </v-app-bar>

      
      <v-navigation-drawer v-model="drawerOpen" expand-on-hover rail theme="dark">
        <v-list>
          <v-list-item prepend-icon="mdi-account" title="Sergio Garcia" subtitle="sergio.garcia84@uabc.edu.mx"></v-list-item>
        </v-list>
        
        <v-divider></v-divider>

        <v-list density="compact" nav>
          <v-list-item prepend-icon="mdi-star" link to="/" color="warning" title="Pagina principal" value="paginaMain" rounded="xl"></v-list-item>
          <v-list-item prepend-icon="mdi-widgets" link to="/imagen" color="info" title="Pagina 1" value="pagina1" rounded="xl"></v-list-item>
          <v-list-item prepend-icon="mdi-check-circle" link to="/tabla" color="error" title="Pagina 2" value="pagina2" rounded="xl"></v-list-item>
          <v-list-item prepend-icon="mdi-forum" link to="/crud" color="warning" title="Pagina 3" value="pagina3" rounded="xl"></v-list-item>

        </v-list>
      </v-navigation-drawer>
      
      <v-main style="height: 1080px">
        <v-card
    class="mx-auto"
    width="400"
    prepend-icon="mdi-home"
  >
    <template v-slot:title>
      PAGINA PARA DESARROLLO WEB
    </template>

    <v-card-text>
      PAGINA 1 - IMAGEN Y TEXTO GENERADO
    </v-card-text>
    <v-card-text>
      PAGINA 2 - TABLA
    </v-card-text>
    <v-card-text>
      PAGINA 3 - TABLA CRUD
    </v-card-text>
    <br>
  </v-card>
        <router-view></router-view>
      </v-main>
    </v-layout>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      drawerOpen: false // Initially drawer is closed
    }
  }
}
</script>


