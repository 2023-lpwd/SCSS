<script>
import MyButton from "./../components/MyButton.vue";
import MyTitle from "./../components/MyTitle.vue";
export default {
  components: {
    MyButton,
    MyTitle,
  },
};
</script>

<template>
  <main>
    <MyTitle label="Titre normale h3" type="h3" />
    <MyTitle size="big" label="Grand Titre h1" />
    <MyTitle size="tiny" type="h2" label="Petit Titre h2" />

    <MyButton label="Cliquez ici !" href="/lien" />
    <MyButton label="Validez le formulaire" type="secondary" size="tiny" />
  </main>
</template>
