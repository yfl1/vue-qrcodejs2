<!--
 * @Description: 
 * @Autor: yfli1
 * @Date: 2020-04-20 15:09:54
 * @LastEditTime: 2020-06-05 21:42:53
 -->
 <template>
  <div class="qrcode" :style="{width:size,height:size}">
    <div id="qrcode" :style="qrcodeStyle" ref="qrcode" style="display:none"></div>
    <canvas ref="mycanvas" width="100" height="100" style="background-color:#eee"></canvas>
  </div>
</template>
<script>
import QRcode from "qrcodejs2";
import logoUrl from "../assets/logo.png";
export default {
  name: "qrcode",
  props: {
    text: {
      type: String,
      default: "52wlp"
    },
    size: {
      type: Number,
      default: 100
    }
  },
  data: function() {
    return {
      qrcode: null
    };
  },
  watch: {
    // text: function() {
    //   if (this.qrcode) {
    //     this.qrcode.clear();
    //     this.qrcode.makeCode(this.text);
    //   }
    // }
  },
  computed: {
    qrcodeStyle: function() {
      return {
        height: this.size + "px",
        width: this.size + "px",
        position: "relative"
      };
    }
  },
  mounted: async function() {
    await this.genQRcode();
    this.drawLogo();
  },
  methods: {
    genQRcode: async function() {
      return new Promise((resolve, reject) => {
        var qrcode = new QRcode(this.$refs.qrcode, {
          text: this.text,
          width: this.size,
          height: this.size,
          colorDark: "#000000",
          colorLight: "#ffffff",
          correctLevel: QRcode.CorrectLevel.H
        });
        if (this.$refs.qrcode.getElementsByTagName("img")[0]) {
          this.qrcode = qrcode;
          resolve();
        }
      });
    },
    drawLogo: function() {
      const rectSize = 40;
      const rectPos = (this.size - rectSize) / 2;
      const logoSize = 30;
      const logoPos = (this.size - logoSize) / 2;
      const logoSrc = logoUrl;

      var ctx = this.$refs.mycanvas.getContext("2d");
      ctx.fillStyle = "#ffffff";

      var qrImg = this.$refs.qrcode.getElementsByTagName("img")[0];
      var logo = new Image();
      logo.src = logoSrc;

      qrImg.onload = function() {
        ctx.drawImage(qrImg, 0, 0, 100, 100);
      };
      logo.onload = function() {
        ctx.fillRect(rectPos, rectPos, rectSize, rectSize);
        ctx.drawImage(logo, logoPos, logoPos, logoSize, logoSize);
      };
    }
  }
};
</script>
<style scoped>
</style>
