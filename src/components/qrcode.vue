<!--
 * @Description: 
 * @Autor: yfli1
 * @Date: 2020-04-20 15:09:54
 * @LastEditTime: 2020-06-05 21:42:53
 -->
 <template>
  <div class="qrcode" :style="{width:size,height:size}">
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
  watch: {},
  computed: {
    qrcodeStyle: function() {
      return {
        height: this.size + "px",
        width: this.size + "px"
      };
    }
  },
  mounted: async function() {
    await this.genQRcode();
    this.drawQrcodeContainLogo();
  },
  methods: {
    genQRcode: async function() {
      return new Promise((resolve, reject) => {
        var element = document.createElement("div");
        var qrcode = new QRcode(element, {
          text: this.text,
          width: this.size,
          height: this.size,
          colorDark: "#000000",
          colorLight: "#ffffff",
          correctLevel: QRcode.CorrectLevel.H
        });
        if (element.getElementsByTagName("canvas")[0]) {
          this.qrcode = element;
          resolve();
        }
      });
    },
    drawQrcodeContainLogo: async function() {
      const rectSize = 40;
      const rectPos = (this.size - rectSize) / 2;
      const logoSize = 30;
      const logoPos = (this.size - logoSize) / 2;
      const qr = await this.getQrImage();
      const logo = await this.getLogoImage();
      var ctx = this.$refs.mycanvas.getContext("2d");
      ctx.fillStyle = "#fff";
      ctx.drawImage(qr, 0, 0, 100, 100);
      ctx.fillRect(rectPos, rectPos, rectSize, rectSize);
      ctx.drawImage(logo, logoPos, logoPos, logoSize, logoSize);
    },
    getQrImage: async function() {
      return new Promise((resolve, reject) => {
        var qr = this.qrcode.getElementsByTagName("img")[0];
        qr.onload = () => {
          resolve(qr);
        };
      });
    },
    getLogoImage: async function() {
      return new Promise((resolve, reject) => {
        var logo = new Image();
        logo.src = logoUrl;
        logo.onload = () => {
          resolve(logo);
        };
      });
    }
  }
};
</script>
<style scoped>
</style>
