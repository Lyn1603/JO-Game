<template>
  <main>
    <header>
      <BugeurMenu/>
      <section class="header">
        <div class="text">
          <div class="text-container">
            <h1>Gagnez des récompenses en revivant les moments forts des JO 2024</h1>

            <p> Participez à notre jeu pour tenter de <b>remporter des cadeaux</b> et revivez
              l'émotion des événements inoubliables des JO.</p>
          </div>
          <div class="button-container">
            <ButtonComponent link="/game" text="Jouer"/>
          </div>
        </div>

        <img src="@/assets/oui.png" alt="" width=50% height=50%>

        <div class="scroll-downs">
          <div class="mousey">
            <div class="scroller"></div>
          </div>
        </div>

      </section>
    </header>
    <section class="section-1">
      <div class="text-container">
        <span>Événement</span>
        <h2>Revivez Paris 2024</h2>
        <p>Redécouvrez l'excitation des Jeux Olympiques de Paris 2024, un événement qui a uni le monde. Accédez
          également à la boutique de l'événement pour vous procurer un éléments exclusif à ce magnifique événement.</p>
        <div class="container-button">
          <a href="https://olympics.com/fr/paris-2024" target="_blank" class="link">En savoir plus</a>
          <a href="https://olympics.com/fr/paris-2024/information/boutique-des-jeux" target="_blank"
             class="link">Boutique</a>
        </div>
      </div>
    </section>
    <section class="section-2">
      <h2>
        Les moment inoubliables
      </h2>
      <div class="history">
        <hr>
        <jo-component
            phryge="https://data.over-blog-kiwi.com/6/42/48/22/20231012/ob_614ed7_natation-marathon-mascotte-oly-2d-rvb.webp"
            sport="Natation"
            date="Léon Marchand"
            video="lGMXC-reWtY"
        ></jo-component>
        <jo-component
            phryge="https://data.over-blog-kiwi.com/6/42/48/22/20231012/ob_f8320e_tennis-de-table-mascotte-oly-2d-rvb.webp"
            sport="Tennis de table"
            date="Les Frère Lebrun"
            video="_qquPg_20Jc"
        ></jo-component>
        <jo-component
            phryge="https://data.over-blog-kiwi.com/6/42/48/22/20231012/ob_8ac9d9_judo-mascotte-oly-2d-rvb.webp"
            sport="Judo"
            date="Teddy Riner"
            video="zW-ELjJvXJw"
        ></jo-component>
      </div>

    </section>
  </main>
</template>

<script>
// @ is an alias to /src
import BugeurMenu from "@/components/BugeurMenu.vue";
import ButtonComponent from "@/components/ButtonComponent.vue";
import JoComponent from "@/components/JoHistory/JoComponent.vue";


export default {
  name: 'HomeView',
  components: {JoComponent, ButtonComponent, BugeurMenu},

}
</script>

<style scoped lang="scss">

main {
}

header {
  padding-top: 100px;
  width: 100%;
  height: 100vh;

  .header {
    position: relative;
    padding: 32px 120px;
    display: flex;
    justify-content: space-around;
    align-items: end;
    height: 100%;
    background-image: url("@/assets/bg-1.png");
    background-size: cover;
    backdrop-filter: blur(32px);
    @media screen and (max-width: 900px) {
      flex-direction: column;
      gap: 32px;
      padding: 0px 20px;
    }

    .button-container{
      width: fit-content;
    }

    img {
      width: 50%;
      object-fit: cover;
      @media screen and (max-width: 900px) {
        height: 50%;
        width: 100%;
      }
    }

    .text-container {
      width:85% ;
    }


    .scroll-downs {
      position: absolute;
      bottom: 16px;
      margin: auto;
      width: 34px;
      height: 55px;
      @media screen and (max-width: 900px) {
        display: none;
      }
    }

    .mousey {
      width: 3px;
      padding: 10px 15px;
      height: 35px;
      border: 2px solid black;
      border-radius: 25px;
      opacity: 0.75;
      box-sizing: content-box;
    }

    .scroller {
      width: 3px;
      height: 10px;
      border-radius: 25%;
      background-color: black;
      animation-name: scroll;
      animation-duration: 2.2s;
      animation-timing-function: cubic-bezier(.15, .41, .69, .94);
      animation-iteration-count: infinite;
    }

    @keyframes scroll {
      0% {
        opacity: 0;
      }
      10% {
        transform: translateY(0);
        opacity: 1;
      }
      100% {
        transform: translateY(15px);
        opacity: 0;
      }
    }


    .text {
      width: 50%;
      display: flex;
      flex-direction: column;
      gap: 32px;


      @media screen and (max-width: 900px) {
        height: 50%;
        width: 100%;
      }

      h1 {
        font-size: 40px;
        font-weight: bold;
        color: #2c3e50;
        @media screen and (max-width: 900px) {
          font-size: 30px;
        }
      }

      p {
        font-size: 20px;
        color: #2c3e50;
        @media screen and (max-width: 900px) {
          font-size: 15px;
        }
      }
    }
  }
}

.section-1 {
  height: 60vh;
  width: 100%;
  background-color: #2c3e50;
  display: flex;
  justify-content: center;
  align-items: center;

  .text-container {
    padding: 0px 120px;
    @media screen and (max-width: 900px) {
      padding: 0px 16px;
    }
  }

  span {
    display: block;
    font-size: 15px;
    padding-top: 10px;
    text-align: left;
    color: white;
    font-weight: bold;
  }

  h2 {
    font-weight: bolder;
    padding: 10px 0px 10px 0px;
    font-size: 50px;
    color: white;
  }

  .link {
    padding: 10px 20px;
    display: inline-block;
    text-decoration: none;
    border: 1px white solid;
    position: relative;
    z-index: 1;
    color: white;
    text-wrap: nowrap;
  }

  .link::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: -1;
    background: none;

    transition: all 0.3s ease-out;
  }

  .link:hover::before {
    transform: scale(1.05);
    background: white;
  }

  .link:hover {
    color: black;
  }

  p {
    color: white;
    padding-bottom: 20px;
  }

  .container-button {
    display: flex;
    align-items: center;
    gap: 16px;
    text-align: center;
    @media screen and (max-width: 400px) {
      flex-direction: column;
      align-items: stretch;
    }
  }
}

.section-2 {
  margin-top: 120px;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 34px 120px;
  @media screen and (max-width: 900px) {
    padding: 0px 24px;
  }

  .history {
    margin-top: 64px;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    gap: 100px;
    flex-wrap: wrap;
    width: 100%;

    hr {
      z-index: -1;
      position: absolute;
      top: 0;
      height: 100%;
      border: 8px solid #2c3e50;
    }
  }

  h2 {
    font-size: 50px;
    font-weight: bold;
    color: #2c3e50;
  }
}


</style>
