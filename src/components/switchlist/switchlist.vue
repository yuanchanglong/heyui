<template>
  <div class="h-switchlist" :class="{'h-switchlist-small':small}" :disabled="disabled">
    <span :checked="option[key]==value" :disabled="disabled" v-for="option of arr" :key="option[key]" @click="setvalue(option[key])">{{option[title]}}</span>
  </div>
</template>
<script>
import config from '../../utils/config';
import utils from '../../utils/utils';

export default {
  name: 'SwitchList',
  props: {
    small: {
      type: Boolean,
      default: false
    },
    datas: [Object, Array],
    disabled: {
      type: Boolean,
      default: false
    },
    dict: String,
    value: [String, Boolean, Number],
    keyName: {
      type: String,
      default: () => config.getOption('dict', 'keyName')
    },
    titleName: {
      type: String,
      default: () => config.getOption('dict', 'titleName')
    }
  },
  data() {
    return {
      key: this.keyName,
      title: this.titleName,
    };
  },
  methods: {
    setvalue(key) {
      if (this.disabled) return;
      if(key == this.value) return;
      this.$emit('input', key);
      let event = document.createEvent("CustomEvent");
      event.initCustomEvent("setvalue", true, true, this.value);
      this.$el.dispatchEvent(event);
    }
  },
  computed: {
    arr() {
      if (!this.datas && !this.dict) {
        log.error('Switchlist组件:datas或者dict参数最起码需要定义其中之一');
        return [];
      }
      let datas = this.datas;
      if (this.dict) {
        datas = config.getDict(this.dict);
      }

      return utils.initOptions(datas, this);
    }
  }
};
</script>
