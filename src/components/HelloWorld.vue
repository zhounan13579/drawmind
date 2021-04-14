<template>
  <el-row :gutter="10">
    <el-col :span="24">
      <el-tooltip effect="dark" content="截屏" placement="top">
        <el-button @click="screenshot" icon="el-icon-crop"></el-button>
      </el-tooltip>
      <el-tooltip effect="dark" content="放大" placement="top">
        <el-button
          icon="el-icon-zoom-in"
          id="zoom-in-button"
          @click="zoomIn"
          ref="zoomIn"
        >
        </el-button>
      </el-tooltip>
      <el-tooltip effect="dark" content="缩小" placement="top">
        <el-button
          icon="el-icon-zoom-out"
          id="zoom-out-button"
          @click="zoomOut"
          ref="zoomOut"
        >
        </el-button>
      </el-tooltip>
      <el-tooltip content="添加节点" placement="bottom">
        <el-button @click="addNode" icon="el-icon-plus"></el-button
        >&nbsp添加节点
      </el-tooltip>
      <el-tooltip content="删除节点" placement="bottom">
<el-button
            icon="el-icon-delete-solid"
            @click="onRemoveNode"
          ></el-button
          >
      </el-tooltip>
      <el-tooltip content="移动到最上方" placement="bottom"> <el-button @click="onMoveUp" icon="el-icon-caret-top"></el-button
          ></el-tooltip>
      <el-tooltip content="移动到最下方" placement="bottom"><el-button @click="onMoveDown" icon="el-icon-caret-bottom"></el-button
          ></el-tooltip>
      <!-- <el-tooltip content="添加图片节点" placement="bottom"><el-button @click="addImageNode" icon="el-icon-picture"></el-button
          ></el-tooltip> -->
      <el-tooltip content="显示数据" placement="bottom"> <el-button
            class="sub"
            @click="get_nodearray_data"
            icon="el-icon-view"
          ></el-button
          ></el-tooltip>
      <el-tooltip content="保存文件" placement="bottom"><el-button class="sub" @click="save_file" icon="el-icon-download">
          </el-button
          ></el-tooltip>
<el-tooltip content="" placement="bottom"><input id="file_input" class="file_input" type="file" /></el-tooltip>
<el-tooltip content="" placement="bottom"><el-button class="sub" @click="open_file" type="success" size="mini">
            打开文件
          </el-button></el-tooltip>
<el-tooltip content="全部展开" placement="bottom"><el-button
            icon="el-icon-circle-plus-outline"
            class="sub"
            @click="expand_all"
          ></el-button
          ></el-tooltip>
<el-tooltip content="全部收缩" placement="bottom"> <el-button
            icon="el-icon-remove-outline"
            class="sub"
            @click="collapse_all"
          ></el-button
          ></el-tooltip>
<el-tooltip content="" placement="bottom"></el-tooltip>


      <el-tooltip effect="dark" content="选择颜色主题" placement="top">
        <el-select v-model="value" @change="set_theme">
          <el-option
            v-for="item in themearray"
            :key="item.label"
            :value="item.value"
          >
          </el-option>
        </el-select>
      </el-tooltip>
    </el-col>
    <el-col :span="24" class="hehe">
      <js-mind
        :values="mind"
        :options="options"
        ref="jsMind"
        height="700px"
        id="jsMind"
      ></js-mind>
    </el-col>
  </el-row>
</template>
<script>
import Vue from "vue";
import jm from "vue-jsmind";
Vue.use(jm);
export default {
  name: "DrawMind",
  components: {},
  mixins: [],
  props: {
    msg: {
      type: String,
      default: "DrawMind",
    },
  },
  data() {
    return {
      jm: null,
      value: "",
      typevalue: "",
      themearray: [
        {
          value: "",
          label: "default（默认）",
        },
        {
          value: "primary",
          label: "primary（主要）",
        },
        {
          value: "warning",
          label: "warning",
        },
        {
          value: "danger",
          label: "danger（危险）",
        },
        {
          value: "info",
          label: "info",
        },
        {
          value: "nephrite",
          label: "nephrite",
        },
        {
          value: "greensea",
          label: "greensea",
        },
        {
          value: "belizehole",
          label: "belizehole",
        },
        {
          value: "wisteria",
          label: "wisteria",
        },
        {
          value: "asphalt",
          label: "asphalt",
        },
        {
          value: "orange",
          label: "orange",
        },
        {
          value: "pumpkin",
          label: "pumpkin",
        },
        {
          value: "pomegranate",
          label: "pomegranate",
        },
        {
          value: "clouds",
          label: "clouds",
        },
        {
          value: "asbestos",
          label: "asbestos",
        },
      ],
      indexspan: true,
      theme_value: "",
      mind: {
        meta: {
          name: "example",
          author: "**@qq.com",
          version: "0.2",
        },
        format: "node_array",
        data: [
          { id: "root", isroot: true, topic: "jsMind" },
          { id: "easy", parentid: "root", topic: "Easy", direction: "left" },
          { id: "easy1", parentid: "easy", topic: "Easy to show" },
          { id: "easy2", parentid: "easy", topic: "Easy to edit" },
          { id: "easy3", parentid: "easy", topic: "Easy to store" },
          { id: "easy4", parentid: "easy", topic: "Easy to embed" },
          {
            id: "open",
            parentid: "root",
            topic: "Open Source",
            expanded: false,
            direction: "right",
          },
          { id: "open1", parentid: "open", topic: "on GitHub" },
          { id: "open2", parentid: "open", topic: "BSD License" },

          {
            id: "powerful",
            parentid: "root",
            topic: "Powerful",
            direction: "right",
          },
          {
            id: "powerful1",
            parentid: "powerful",
            topic: "Base on Javascript",
          },
          { id: "powerful2", parentid: "powerful", topic: "Base on HTML5" },
          { id: "powerful3", parentid: "powerful", topic: "Depends on you" },
        ],
      },
      options: {
        mode: "side",
      },
      shortCutVal: "",
      keyCode: "",
      iconchange: "el-icon-circle-close",
      color: "black",
      bgcolor: "white",
      num: 15,
      numoptions: [
        { value: 8, item: 8 },
        { value: 10, item: 10 },
        { value: 12, item: 12 },
        { value: 14, item: 14 },
        { value: 16, item: 16 },
        { value: 18, item: 18 },
        { value: 20, item: 20 },
        { value: 22, item: 22 },
      ],
      shortCutVal: "",
      keyCode: "",
    };
  },
  mounted() {
    this.jm = this.$refs.jsMind.jm;
    this.jm.enable_edit();
  },
  methods: {
    expand_all() {
      this.jm.expand_all();
    },
    collapse_all() {
      this.jm.collapse_all();
    },
    open_file() {
      let file_input = document.getElementById("file_input");
      let files = file_input.files;
      let _this = this;
      if (files.length > 0) {
        let file_data = files[0];
        jsMind.util.file.read(file_data, function(jsmind_data, jsmind_name) {
          let mind = jsMind.util.json.string2json(jsmind_data);
          if (!!mind) {
            _this.jm.show(mind);
          } else {
            _this.$message.error("请选择正确的文件及数据格式");
          }
        });
      } else {
        this.$message("请先选择文件");
      }
    },
    save_file() {
      let mind_data = this.jm.get_data();
      let mind_name = mind_data.meta.name;
      let mind_str = jsMind.util.json.json2string(mind_data);
      jsMind.util.file.save(mind_str, "text/jsmind", mind_name + ".jm");
    },
    get_nodearray_data() {
      let mind_data = this.jm.get_data("node_array");
      let mind_string = jsMind.util.json.json2string(mind_data);
      alert(mind_string);
    },
    shortcutSet(value) {
      if (value.key === "Backspace" || value.key === "Delete") {
        this.shortCutVal = this.shortCutVal.substring(
          0,
          this.shortCutVal.lastIndexOf("+")
        );
        this.keyCode = this.keyCode.substring(0, this.keyCode.lastIndexOf("+"));
        return;
      }
      if (this.shortCutVal) {
        this.shortCutVal += " + ";
        this.keyCode += "+";
      }
      this.shortCutVal += value.key;
      this.keyCode += value.keyCode;
      // console.log("keyCode", this.keyCode);
      this.options = {
        shortcut: {
          mapping: {
            // 快捷键映射
            addchild: this.keyCode,
          },
        },
      };
    },
    change_text_font() {
      let selected_id = this.get_selected_nodeid();
      if (!selected_id) {
        this.$message("请选择一个节点.");
        return;
      }
      this.jm.set_node_font_style(selected_id, this.num);
    },
    change_text_color() {
      let selected_id = this.get_selected_nodeid();
      if (!selected_id) {
        this.$message("请选择一个节点.");
        return;
      }
      this.jm.set_node_color(selected_id, null, this.color);
    },
    change_background_color() {
      let selected_id = this.get_selected_nodeid();
      if (!selected_id) {
        this.prompt_info("请选择一个节点.");
        return;
      }
      this.jm.set_node_color(selected_id, this.bgcolor, null);
    },
    togglerightspan() {
      this.indexspan = !this.indexspan;
      if (this.indexspan == true) {
        Vue.set(this.span, 0, 18);
        Vue.set(this.span, 1, 5);
      } else {
        Vue.set(this.span, 0, 23);
        Vue.set(this.span, 1, 0);
      }
    },
    addNode() {
      var selected_node = this.jm.get_selected_node(); // as parent of new node
      if (!selected_node) {
        alert("请先选择一个节点");
        return;
      }

      var nodeid = jsMind.util.uuid.newid();
      var topic = "new Node";
      var node = this.jm.add_node(selected_node, nodeid, topic);
    },
    onMoveUp() {
      var selected_id = this.jm.get_selected_node();
      if (!selected_id) {
        alert("请先选择一个节点");
        return;
      }
      this.jm.move_node(selected_id, "_first_");
    },
    onMoveDown() {
      var selected_id = this.jm.get_selected_node();
      if (!selected_id) {
        alert("请先选择一个节点");
        return;
      }

      this.jm.move_node(selected_id, "_last_");
    },
    onRemoveNode() {
      var selected_id = this.get_selected_nodeid();
      if (!selected_id) {
        alert("请先选择一个节点");
        return;
      }
      this.jm.remove_node(selected_id);
    },
    addImageNode() {
      var imageChooser = document.getElementById("image-chooser");
      const _this = this;
      imageChooser.addEventListener(
        "change",
        function(event) {
          var reader = new FileReader();
          reader.onloadend = function() {
            var selected_node = _this.jm.get_selected_node();
            var nodeid = jsMind.util.uuid.newid();
            var topic = undefined;
            var data = {
              "background-image": reader.result,
              width: "100",
              height: "100",
            };
            var node = _this.jm.add_node(selected_node, nodeid, topic, data);
          };

          var file = imageChooser.files[0];
          if (file) {
            reader.readAsDataURL(file);
          }
        },
        false
      );
      var selected_node = this.jm.get_selected_node(); 
      if (!selected_node) {
      alert("请先选择一个节点");
        return;
      }

      imageChooser.focus();
      imageChooser.click();
    },
    openLocalFile() {
      var file_input = this.$refs.input;
      var files = file_input.files;
      const _this = this;
      if (files.length > 0) {
        var file_data = files[0];
        jsMind.util.file.read(file_data, function(jsmind_data, jsmind_name) {
          var mind = jsMind.util.json.string2json(jsmind_data);
          if (!!mind) {
            _this.mind = mind;
            _this.jm.show(mind);
          } else {
            alert("can not open this file as mindmap");
          }
        });
      } else {
        alert("please choose a file first");
      }
    },
    saveLocalFile() {
      var mind_data = this.jm.get_data();
      var mind_name = mind_data.meta.name;
      var mind_str = jsMind.util.json.json2string(mind_data);
      jsMind.util.file.save(mind_str, "text/jsmind", mind_name + ".jm");
    },
    fontSize() {
      var selected_id = this.get_selected_nodeid();
      if (!selected_id) {
        alert("请先选择一个节点");
        return;
      }
      this.jm.set_node_font_style(selected_id, 28);
    },
    fontColor() {
      var selected_id = this.get_selected_nodeid();
      if (!selected_id) {
        alert("请先选择一个节点");
        return;
      }
      this.jm.set_node_color(selected_id, null, "#000");
    },
    bgColor() {
      var selected_id = this.get_selected_nodeid();
      if (!selected_id) {
        alert("请先选择一个节点");
        return;
      }

      this.jm.set_node_color(selected_id, "#eee", null);
    },
    bgImage() {
      var selected_id = this.get_selected_nodeid();
      if (!selected_id) {
        alert("请先选择一个节点");
        return;
      }
      this.jm.set_node_background_image(selected_id, "ant.png", 100, 100);
    },
    set_theme() {
      // console.log(this.value);

      this.jm.set_theme(this.value);
    },
    set_type() {
      this.$message("该功能待完善");
    },
    zoomOut() {
      if (this.jm.view.zoomOut()) {
        this.$refs.zoomOut.disabled = false;
      } else {
        this.$refs.zoomOut.disabled = true;
      }
    },
    zoomIn() {
      if (this.jm.view.zoomIn()) {
        this.$refs.zoomIn.disabled = false;
      } else {
        this.$refs.zoomIn.disabled = true;
      }
    },
    screenshot() {
      this.jm.screenshot.shootDownload();
    },
    // 获取选中标签的 ID
    get_selected_nodeid() {
      var selected_node = this.jm.get_selected_node();
      if (!!selected_node) {
        return selected_node.id;
      } else {
        return null;
      }
    },
    changeOption() {
      this.options = {
        mode: "",
      };
    },
    // 只支持绑定单个按键
    shortcutSet(value) {
      if (value.key === "Backspace" || value.key === "Delete") {
        this.shortCutVal = this.shortCutVal.substring(
          0,
          this.shortCutVal.lastIndexOf("+")
        );
        this.keyCode = this.keyCode.substring(0, this.keyCode.lastIndexOf("+"));
        return;
      }
      if (this.shortCutVal) {
        this.shortCutVal += " + ";
        this.keyCode += "+";
      }
      this.shortCutVal += value.key;
      this.keyCode += value.keyCode;
      // console.log("keyCode", this.keyCode);
      this.options = {
        shortcut: {
          mapping: {
            // 快捷键映射
            addchild: this.keyCode,
          },
        },
      };
    },
  },
};
</script>

<style scoped>
li {
  list-style: none;
}
</style>
