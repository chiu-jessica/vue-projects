 <template>
    <div id = "quiz-result">
        <div class = "result-wrapper">
            <img :src = "characterImgs[character]">
            <p>You are <span class = "character-name">{{character}}</span></p>
        </div>
    </div>
 </template>

 <script>
    export default {
        data () {
            return {
                character: null,
                characterImgs: {
                    rocket: "https://upload.wikimedia.org/wikipedia/en/c/ce/Rocket_in_Avengers_Endgame.jpeg",
                    starlord: "https://akns-images.eonline.com/eol_images/Entire_Site/20201114/rs_1200x1200-201214124949-1200-Chris_Pratt-Guardians_Of_The_Galaxy_-_2014.jpg?fit=around%7C1080:1080&output-quality=90&crop=1080:1080;center,top", 
                    gamora: "https://upload.wikimedia.org/wikipedia/en/thumb/5/54/Zoe_Saldana_as_Gamora.jpeg/220px-Zoe_Saldana_as_Gamora.jpeg", 
                    drax: "https://upload.wikimedia.org/wikipedia/en/3/3d/Dave_Bautista_as_Drax.jpg"
                }
            }
        },
        mounted () {
            var x = new URLSearchParams(window.location.search)
            if (x.has("result")) {
                this.character = x.get("result").toLowerCase()
            } else {
                window.location.href = "/quiz"
            }
        }
    }
</script>

<style lang="scss">
    #quiz-result {
        background-color:#90c796;
        width: 100vw;
        height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        .result-wrapper {
            background-color: white;
            width: 400px;
            height: 400px;
            padding: 40px;
            img {
                width: 100%;
                height: calc(100% - 40px);
                object-fit: cover;
            }
            .character-name {
                text-transform: capitalize;
            }
        }
    }
</style>