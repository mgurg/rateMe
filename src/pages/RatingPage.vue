<template>
  <div class="row justify-center">
    <q-page class="col-lg-8 col-sm-10 col-xs q-pa-xs">
      <RatingGrid v-if="rating===null" @rating-set="parseRating"/>

      <div class="col-12 q-py-lg">
        <div v-if="rating && rating > 3">
          <div class="row justify-center q-py-lg text-h5">Super ❤️ </div>
          <div class="row justify-center q-py-lg q-px-md text-body1">Podziel się swoją opinią na Google Maps i pokaż innym, że warto do nas wpaść. To zajmie Ci tylko minutę!</div>
          <div class="row justify-center q-py-lg">
            <q-btn color="primary" outline @click="redirect">&nbsp; Podziel się opinią z innymi</q-btn>
          </div>
        </div>

        <div v-if="rating && rating <4">
          <div class="row justify-center">

            <q-rating
              v-model="ratingModel"
              :color-selected="ratingColors"
              class="row justify-center q-py-lg"
              color="blue-grey-9"
              icon="star_border"
              icon-selected="star"
              size="3.5em"
            />
          </div>
<!--          <div class="row justify-center q-py-lg">😥 Co możemy zrobić lepiej️?</div>-->
          <q-input
            v-model="text"
            class="full-width row"
            outlined
            type="textarea"
            label="Podziel się swoimi doświadczeniami"
          />
          <div class="row q-pt-lg">
            <q-space/>
            <q-btn
              label="Anuluj"
              class="q-mr-lg"
              color="red-12"
              flat
              icon="cancel"
              type="submit"
            />

            <q-btn
              label="Wyślij"
              class="q-mr-xs"
              color="primary"
              icon="done"
              type="submit"
              @click="webhookNotification()"
            />
          </div>
        </div>


      </div>

    </q-page>
  </div>
</template>

<script setup>
import {ref} from "vue";
import {useDiscordWebhook} from "src/composables/useDiscordWebhook";
import RatingGrid from "components/RatingGrid.vue";

const rating = ref(null)
const ratingModel = ref(0)
const ratingColors = ref(['amber-8', 'amber-8', 'amber-8', 'amber-8', 'amber-8']);
const text = ref(null)

function redirect() {
  window.location.href = process.env.VUE_REMOTE_URL
}

function redirectOutside() {
  window.location.href = process.env.VUE_COMPANY_URL
}

function parseRating(params) {
  rating.value = params.value
}

async function webhookNotification() {

  const {error} = await useDiscordWebhook("Rating: " + '⭐'.repeat(ratingModel.value) + '\nDescription: ' + text.value + '\n')

  redirectOutside()

}


</script>
