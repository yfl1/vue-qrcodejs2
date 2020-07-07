<!--
 * @Description: 
 * @Autor: yfli1
 * @Date: 2020-04-20 15:09:54
 * @LastEditTime: 2020-06-05 21:42:53
 -->
 <template>
  <div class="qrcode" :style="{width:size,height:size}">
    <div id="qrcode" :style="qrcodeStyle" ref="qrcode"></div>
  </div>
</template>
<script>
import QRcode from "qrcodejs2";
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
        position: "static"
      };
    }
  },
  mounted: function() {
    this.genQRcode();
  },
  methods: {
    genQRcode: function() {
      var qrcode = new QRcode(this.$refs.qrcode, {
        text: this.text,
        width: this.size,
        height: this.size,
        colorDark: "#000000",
        colorLight: "#ffffff",
        correctLevel: QRcode.CorrectLevel.H
      });
      this.qrcode = qrcode;
    }
  }
};
</script>
<style scoped>
</style>
