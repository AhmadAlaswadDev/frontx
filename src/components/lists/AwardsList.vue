<template>
  <div class="">
    <b-card class="my-3 px-2">
      <b-row class="justify-content-between align-items-start">
        <h4 class="text-start card-title mb-4 p-1 font-weight-bold">Award</h4>
        <b-button
          variant="link"
          class="font-weight-bold h5 text-dark"
          v-b-toggle.collapse-aw
        >
          + Add Award</b-button
        >
      </b-row>
      <b-row>
        <b-collapse id="collapse-aw" class="mt-2" style="flex: 1">
          <award v-bind:CvId="cvOne.cvId" v-bind:type="'newItem'"></award>
        </b-collapse>
      </b-row>
    </b-card>
    <div class="">
      <draggable
        v-bind="dragOptions"
        handle=".handle"
        group="people"
        @start="drag = true"
        @end="DragEnd()"
        v-model="draglist"
      >
        <transition-group type="transition" :name="!drag ? 'flip-list' : null">
          <b-card
            class="my-3 p-2"
            v-for="aw in this.draglist"
            v-bind:key="aw._id"
          >
            <div class="d-flex justify-content-between">
              <b-icon icon="justify" class="h1 handle"></b-icon>
              <h4>{{ aw.AwTitle }}</h4>
              <div class="">
                <b-button v-b-toggle="'collap' + aw._id"> colp </b-button>
                <b-button @click="DeleteAwSubmit(aw._id)" variant="danger"
                  >Del</b-button
                >
              </div>
            </div>
            <b-collapse :id="'collap' + aw._id">
              <award
                v-bind:CvId="cvOne.cvId"
                v-bind:award="aw"
                v-bind:type="'item'"
              ></award>
            </b-collapse>
          </b-card>
        </transition-group>
      </draggable>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import award from "../items/award.vue";
import draggable from "vuedraggable";

export default {
  components: {
    award,
    draggable,
  },
  computed: {
    ...mapGetters(["awards", "cvOne"]),
    dragOptions() {
      return {
        animation: 200,
        group: "description",
        disabled: false,
        ghostClass: "ghost",
      };
    },
  },
  watch:{

    cvOne(){
      console.log(' award chnaged after delete from award list')
    }

  },
  data() {
    return {
      drag: false,
      draglist:[]
    };
  },
  methods: {
    ...mapActions(["deleteAw","changeAw"]),
    DeleteAwSubmit: function (awid) {
      this.deleteAw(awid);
    },
    DragEnd() {
      this.drag = false;
      console.log("drag end");
      console.log(this.draglist)
      this.changeAw({list:this.draglist,CvId:this.cvOne.cvId})
    },
  },
  mounted(){
    this.draglist=this.awards;
  }
};
</script>

<style></style>
