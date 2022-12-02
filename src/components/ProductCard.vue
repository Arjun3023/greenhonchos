<template>
  <v-container>
    <template>
      <v-hover v-slot="{ hover }">
        <v-card flat class="mx-auto" max-width="344">
          <v-img :src="item.image"></v-img>

          <v-card-text class="text--primary">
            <p class="subtitle-1">{{ item.name }}</p>
            <p class="subtitle-1">Rs. {{ item.selling_price }}</p>
          </v-card-text>

          <v-card-actions v-if="hover">
            Size-
            <span
              v-for="size in Object.keys(item.variation)"
              :key="size.id_product"
              >&nbsp;{{ size }}
            </span>
          </v-card-actions>
        </v-card>
      </v-hover>
    </template>
  </v-container>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
    },
  },
};
</script>

<style></style>
