<script setup>

  var stringQuery = ref("");
  var result = ref("");

const payload = {
  "username": 'Eleonore',
  "email": 'eleonore.voisin@mail.numerique.gouv.fr',
  "password": "kC4rDTPX8xjuN"
}

const response = await useFetch('https://albert.etalab.gouv.fr/api/v2/sign_in', {
    method: 'POST',
    body: {
      ...payload
    }
});

var token = response.data.value.token;

// var token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE3MTQ4MDc0MTQsImlhdCI6MTcxNDcyMTAxNCwic3ViIjoiMjE2In0.3Y3OZFJmxAY22dANyjRQTpWoJJxBqA6NdQbNz-GxI48";

console.log(token);

const data = await useFetch('https://albert.etalab.gouv.fr/api/v2/chat', {
    method: 'POST',
    headers: {
      authorization: `Bearer ${token}`,
    },
    body : {
      "chat_type": "qa"
    }
});

const chatId = data.data.value.id;

const search = async function() {
var token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE3MTQ4MDc0MTQsImlhdCI6MTcxNDcyMTAxNCwic3ViIjoiMjE2In0.3Y3OZFJmxAY22dANyjRQTpWoJJxBqA6NdQbNz-GxI48";

  const streamChat = await useFetch(`https://albert.etalab.gouv.fr/api/v2/stream/chat/${chatId}`, {
      method: 'POST',
      headers: {
        authorization: `Bearer ${token}`,
      },
      body: {
        "query": this.stringQuery,
        "temperature": 20,
        "model_name": "AgentPublic/albertlight-7b",
        "limit": 7,
        "mode": "rag",
        "sources": [
          "service-public",
          "travail-emploi"
        ],
        "must_not_sids": [],
        "with_history": true
      }
  });

  console.log(streamChat);

  const streamId = streamChat.data.value.id;
  if (streamId) {
    const indexes = await useFetch(`https://albert.etalab.gouv.fr/api/v2/stream/${streamId}`);
    this.result = indexes;
  }
}


 


// console.log(streamChat);
// console.log(responseIndexes);


</script>

<template>
  <div class="fr-container fr-mb-10w fr-mt-10w">
<div class="fr-input-group">
    <label class="fr-label" for="text-input-groups1">Ecris un truc</label>
    <input class="fr-input" :v-model="stringQuery" type="text" id="text-input-groups1" name="text-input-groups1">
    <button class="fr-btn fr-mt-2w" @click="search()">
    Allez ! 
</button>
</div>


    {{ result }}
  </div>
</template>
