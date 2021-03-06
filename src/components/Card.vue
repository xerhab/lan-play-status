<template>
  <div class="card">
    <img
      :src="
        game.source
          ? game.source
          : game.asset
          ? require(`@/assets/games/${game.asset}`)
          : image
      "
      class="card__image"
      alt=""
    />
    <div class="card__content">
      <h2>{{ game.title }}</h2>
      <p v-if="message">{{ game.message }}</p>
    </div>
  </div>
</template>

<script>
const capitalize = ([first, ...rest]) =>
  first ? first.toUpperCase() + rest.join("").toLowerCase() : "";

export default {
  props: {
    game: Object
  },
  computed: {
    image() {
      let title = this.game.title
        // Remove diactrics
        .normalize("NFD")
        .replace(/[\u0300-\u036f]/g, "")
        // Fix strange names
        .replace(/&/g, "and")
        .replace(/'n'/g, "n ")
        .replace(/ 'EM/g, "em")
        // Remove useless chars
        .replace(/:|'|®|™|!/g, "")
        // Replace delimiter by space
        .replace(/-|\.|\//g, " ")
        // Capitalize
        .split(" ")
        .map(s => capitalize(s))
        .join("")
        // Fix roman numbers
        .replace("Iii", "III")
        .replace("CivilizationVi", "CivilizationVI")
        // Fix specifics namings
        .replace("Ark", "ARK")
        .replace("ark", "ARK")
        .replace("MARKed", "Marked")
        .replace("Rmx", "RMX")
        .replace("Dx", "DX")
        .replace("Korg", "KORG")
        .replace("Nba2k", "NBA2K")
        .replace("Gp18", "gp18")
        .replace("SuperDragonBall", "Superdragonball")
        .replace("Gp1", "GP1")
        .replace("Gp2", "GP2")
        .replace("Snk", "SNK")
        .replace("Fighterz", "FighterZ")
        .replace("EaSportsFifa1", "EASportsFifa1")
        .replace("ChikiChikiBoxyRacers", "_ChikiChikiBoxyRacers")
        .replace("Mudrunner", "MudRunner")
        .replace("ReMarsTered", "ReMarstered");

      if (this.game.lang) {
        title = `${title}_${this.game.lang}`;
      }

      return `https://cdn01.nintendo-europe.com/media/images/11_square_images/games_18/nintendo_switch_${
        this.game.ds ? "download_software" : "5"
      }/SQ_NSwitch${this.game.ds ? "DS" : ""}_${title}_image500w.jpg`;
    }
  }
};
</script>

<style lang="scss">
.card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
  border-radius: 5px;
  width: 300px;
  margin-bottom: 20px;

  @media only screen and (max-width: 768px) {
    width: 150px;
  }

  &__image {
    border-radius: 5px 5px 0 0;
    max-width: 100%;
  }

  &:hover {
    box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
  }

  &__content {
    padding: 2px 16px;
  }
}
</style>
