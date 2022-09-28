<template>
  <div id="pantone-container">
    <div id="pantone-card" @click="copyHexToClipboard(pantone)">
    <div id="container">
        <div>
        <div id="color-box" :style="pantoneStyle">
        </div>

        <div id="pantone-name">
            <h4>{{pantone.name.replaceAll('-', ' ')}}</h4>
            <h5>{{`#${pantone.hex.toUpperCase().replaceAll(',', '')}`}}</h5>
        </div>
    </div>
    </div>
  </div>
  </div>
</template>

<script>
export default {
    props: ['pantone'],
    data() {
        return {
            pantoneStyle: {
                backgroundColor: `#${this.pantone.hex.replaceAll(',', '')}`
            }
        }
    },
    methods: {
        async copyHexToClipboard(pantone) {
            let textArea = document.createElement("textarea");
            textArea.value = `#${pantone.hex}`;
            textArea.style.position = "fixed";
            textArea.style.left = "-999999px";
            textArea.style.top = "-999999px";
            document.body.appendChild(textArea);
            textArea.focus();
            textArea.select();
            return new Promise((res, rej) => {
            // here the magic happens
            document.execCommand('copy') ? res() : rej();
            textArea.remove();
            document.getElementById('page-title').innerText = "Copy Pasted!"
            setTimeout(() => {
                document.getElementById('page-title').innerText = "色APP"
            }, 5000);
            

        });
        }
    }
}
</script>

<style scoped>

    #pantone-container {
        width: calc(100% * (1/10) - 10px - 1px);

    }

#pantone-card {
    /* Margin & Display  */
    padding-bottom: 5em;
    margin: 0.5em 0.5em 4vh;
    display: inline-block;

    /* Border Styling  */
    border-color: black;
    border-style: solid;

    /* Sizing */
    width: 100px;
    height: 120px;
}
#color-box {

    /* Sizing */
    height: 100px;
    width: 100px;
}

</style>