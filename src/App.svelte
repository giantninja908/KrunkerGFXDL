<script lang="ts">
  import { invoke } from "@tauri-apps/api/tauri";
  import ActiveWeapon from "./ActiveWeapon.svelte";
  import Display from "./Display.svelte";
  import UserInput from "./UserInput.svelte";

  const weapontype = {
    WEAPON: {
      SNIPER: "weapon/weapon_1",
      AK: "weapon/weapon_2",
      PISTOL: "weapon/weapon_3",
      SMG: "weapon/weapon_4",
      REVOLVER: "weapon/weapon_5",
      SHOTGUN: "weapon/weapon_6",
      LMG: "weapon/weapon_7",
      SEMI: "weapon/weapon_8",
      RL: "weapon/weapon_9",
      AKIMBO: "weapon/weapon_10",
      DEAGLE: "weapon/weapon_11",
      ALIEN: "weapon/weapon_12",
      ALIEN_OLD: "weapon/weapon_13",
      CROSSBOW: "weapon/weapon_14",
      FAMAS: "weapon/weapon_15",
      SAWED_OFF: "weapon/weapon_16",
      AUTO_PISTOL: "weapon/weapon_17",
      BOMB: "weapon/weapon_18",
      BLASTER: "weapon/weapon_19",
      BUILD: "weapon/weapon_20",
      SHIELD: "weapon/weapon_21",
    },
    HAT: "hats/hat_",
    BODY: "body/body_",
    MELEE: "melee/melee_",
    SPRAYS: "spray/",
    DYES: "dye/",
    WAIST: "waist/waist_",
    FACE: "faces/face_",
    SHOE: "shoes/shoe_",
    PETS: "pets/pet_",
  };

  let selwep = {
    preview: "",
    name: "",
    season: "",
    type: "",
  };

  let weapons = [];
  const types = [
    "weapons/weapon_",
    "hats/hat_",
    "body/body_",
    "melee/melee_",
    "sprays/",
    "dyes/",
  ];
  const getPrev = (a) => {
    return (
      "https://assets.krunker.io/textures/" +
      (a["type"] && 0x4 == a["type"] && !a["is3D"]
        ? "sprays/" + a["id"]
        : "previews/" +
          (a["type"] && 0x3 != a["type"]
            ? "cosmetics/" +
              a["type"] +
              "_" +
              a["id"] +
              (a["tex"] ? "_" + a["tex"] : "")
            : types[a["type"] || 0x0] +
              (a["type"] && 0x3 == a["type"]
                ? a["id"] +
                  (null == a["pat"]
                    ? null == a["tex"]
                      ? ""
                      : "_" + a["tex"]
                    : "_c" + a["pat"])
                : (a["weapon"] || 0x0) +
                  "_" +
                  (null == a["mid"]
                    ? null == a["pat"]
                      ? a["tex"]
                        ? a["tex"]
                        : a["id"]
                      : "c" + a["pat"]
                    : "m" +
                      a["mid"] +
                      (null == a["midT"] ? "" : "_" + a["midT"]))))) +
      ".png"
    );
  };

  let selweps = [];
  let selwepIndex = 0;
  const searchFor = async (event) => {
    selwepIndex = 0;
    selweps = weapons.filter((e) =>
      e.name.toLowerCase().startsWith(event.detail.searchVal.toLowerCase())
    );
    selwep = selweps[selwepIndex];
  };

  fetch("http://krunkreqapi.person_with_a_b.repl.co/char4Dat.json")
    .then((e) => e.json())
    .then((e) => {
      weapons = e.map((elem) => {
        let preview = getPrev(elem);
        let urls = {};

        if (elem.type === undefined) {
          if (elem.hasOwnProperty("pat")) {
            urls[
              "texture"
            ] = `https://assets.krunker.io/textures/weapons/pat/${elem.pat}.png`;
          } else if (elem.hasOwnProperty("mid")) {
            urls[
              "texture"
            ] = `https://assets.krunker.io/textures/weapons/weapon_${
              elem.weapon
            }${
              elem.hasOwnProperty("midT") ? "_" + elem.midT : "_" + elem.mid
            }.png`;
          } else {
            urls[
              "texture"
            ] = `https://assets.krunker.io/textures/weapons/skins/weapon_${elem.weapon}_${elem.id}.png`;
          }
          urls["model"] = `https://assets.krunker.io/models/weapons/weapon_${
            elem.weapon
          }${elem.hasOwnProperty("mid") ? "_" + elem.mid : ""}.obj`;
        } else {
          urls["texture"] = `https://assets.krunker.io/textures/${
            types[elem.type]
          }${elem.id}${elem.tex !== undefined ? "_" + elem.tex : ""}.png`;
          if (![4, 5].includes(elem.type)) {
            urls["model"] = `https://assets.krunker.io/models/${
              types[elem.type]
            }${elem.id}.obj`;
          }
        }

        return {
          preview,
          name: elem.name,
          id: elem.id,
          season: elem.seas,
          creator: elem.creator,
          urls,
        };
      });
      console.log(e);
    });

  const back = () => {
    selwepIndex--;
    if (selwepIndex < 0) {
      selwepIndex = selweps.length - 1;
    }
  };
  const next = () => {
    selwepIndex++;
    if (selwepIndex > selweps.length - 1) {
      selwepIndex = 0;
    }
  };
</script>

<main>
  <Display>
    {#if selweps.length > 0}
      <button on:click={back} class="lastbtn">LAST</button>
      <ActiveWeapon data={selweps[selwepIndex]} />
      <button on:click={next} class="nextbtn">NEXT</button>
    {/if}
  </Display>
  <UserInput on:update={searchFor} />
</main>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
  }
  main {
    text-align: center;
    max-width: 240px;
    margin: 0;
    padding: 0;
    background-color: #01030f;
    overflow: hidden;
    min-height: 100vh;
    color: #fff;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }

  .lastbtn,
  .nextbtn {
    height: auto;
    width: 5em;
    margin: 2em;
  }
</style>
