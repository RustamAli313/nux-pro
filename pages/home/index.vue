<template>

<div light>
  <h1 class="display-4">home page</h1>
  <p class="red green--text lighten-4 text--darken-3">Lorem ipsum dolor sit amet consectetur adipisicing elit. Animi eligendi dolorum aspernatur nostrum harum itaque rem, ducimus error, obcaecati voluptatem hic, maxime unde nobis quasi magnam voluptate ex necessitatibus vero.</p>
  <h3>This is h3 font</h3>
  <p class="heading">this is a heading</p>
  <p class="subheading red--text text-lowercase font-weight-black ">this is a subheading</p>
  <v-btn class="pink white--text" >Click Me</v-btn>
  <v-btn small color="pink">Click Me</v-btn>
  <v-btn depressed color="pink">Click Me</v-btn>
  <v-btn flat class="pink">Click Me fast</v-btn>
  <v-btn flat class="pink">
    <v-icon fab left dark>
        mdi-pencil
      </v-icon>
  </v-btn>

  <v-btn
      class="mx-2"
      fab
      dark
      color="indigo"
    >
      <v-icon dark>
        mdi-plus
      </v-icon>
    </v-btn>

    <v-btn class="mx-2" fab dark color="primary" >
    <v-icon >
      mdi-heart
      </v-icon>
    </v-btn>

    <v-btn class="hidden-sm-and-down" >
      I will hide
    </v-btn>

</div>
</template>
