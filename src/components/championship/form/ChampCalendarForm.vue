<template>
  <div>
    <v-label>Add Stages</v-label>
    <StageItemForm
      v-for="(stage, i) in stages"
      :key="i"
      :_isLast="stagesCount - 1 == i"
      :_index="stage.index"
      :_stage="stage"
      @addStage="addStage"
      @updateStage="updateStage"
      @removeStage="removeStage"
    />
    <v-btn :disabled="!isValid" color="primary" @click="nextStep">Continue</v-btn>
    <v-btn flat @click="$emit('backStep')">Back</v-btn>
  </div>
</template>

<script>
import StageItemForm from '@/components/form-elements/StageItemForm.vue'
export default {
  name: 'ChampCalendarForm',
  data() {
    return {
      stages: [
        {
          track: '',
          date: null,
          time: null,
          index: Math.random()
        }
      ]
    }
  },
  watch: {
    // stages: {
    //   handler: function(newValue) {
    //     this.nextStep(newValue)
    //   },
    //   deep: true
    // }
  },
  computed: {
    stagesCount() {
      return this.stages.length
    },
    isValid() {
      var isValid = false
      this.stages.forEach((stage, i) => {
        if (!stage.track || !stage.date || !stage.time) {
          isValid = false
        } else {
          isValid = true
        }
      })
      return isValid
    }
  },
  methods: {
    nextStep() {
      this.$emit('nextStep', this.stages, 'calendar')
    },
    addStage() {
      var stage = {
        track: '',
        date: null,
        time: this.stages.time,
        index: Math.random()
      }
      this.stages.push(stage)
    },
    updateStage(stage) {
      let arayIndex = null
      this.stages.forEach((item, i, arr) => {
        if (item.index == stage.index) {
          arayIndex = i
        }
      })
      this.stages.splice(arayIndex, 1, stage)
    },
    removeStage(id) {
      if (this.stages.length !== 1) {
        let index = null
        this.stages.forEach((item, i, arr) => {
          if (item.id == id) {
            index = i
          }
        })
        this.stages.splice(index, 1)
      }
    }
  },
  components: {
    StageItemForm
  }
}
</script>