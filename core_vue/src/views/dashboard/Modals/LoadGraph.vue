<template>
  <b-modal title="Load Graph" size="lg" v-model="myModal" @ok="handleOk()" @cancel="hide()" hide-header-close >
    <div class="row">
      <div class="col-sm-2" style="border-right: 1px solid #a4b7c1">
        <b-list-group>
          <b-list-group-item id="root" v-on:click="openView('rootgraphs')" href="#" view="rootgraphs" class="sections btn-secondary btn-sm active">Templates</b-list-group-item>
          <b-list-group-item id="user" v-on:click="openView('usergraphs')" href="#" view="usergraphs" class="sections btn-secondary btn-sm">Saved</b-list-group-item>
        </b-list-group>
      </div>
      <div id="rootgraphs" class="col-sm-10 graphviews">
        <div class="col-sm-6">
          <select name="user" size="8" class="form-control">
            <template v-for='graph in rootgraphs'>
              <option :value=graph.graph_id>{{graph.title}}</option>
            </template>
          </select>
        </div>
      </div>
      <div  id="usergraphs" class="col-sm-10 graphviews hiddenView">
        <div class="col-sm-6">
          <select name="user" size="8" class="form-control">
            <template v-for='graph in usergraphs'>
              <option :value=graph.graph_id>{{graph.title}}</option>
            </template>
          </select>
        </div>
      </div>
    </div>
  </b-modal>
</template>

<script>
import { mapGetters, mapMutations, mapActions } from 'vuex'
import $ from 'jquery'
// import utils from '../_utils'

export default {
  name: 'loadGraph',
  data () {
    return {
      graphId: ''
    }
  },
  computed: {
    ...mapGetters({
      visible: 'loadGraphVisible',
      rootgraphs: 'graph/rootgraphs',
      usergraphs: 'graph/usergraphs',
      cy: 'cy'
    }),
    myModal: {
      get: function () {
        return this.visible
      },
      set: function (value) {
        if (value === true) {
          this.show()
        } else {
          this.hide()
        }
      }
    }
  },
  methods: {
    ...mapMutations({
      show: 'showLoadGraph',
      hide: 'hideLoadGraph'
    }),
    ...mapActions({
      updateRootGraphs: 'graph/updateRootGraphs',
      updateUserGraphs: 'graph/updateUserGraphs',
      setCurrentGraphType: 'graph/setCurrentGraphType',
      setCurrentGraphId: 'graph/setCurrentGraphId',
      setCurrentGraphTitle: 'graph/setCurrentGraphTitle'
    }),
    handleOk: function () {
      let val = $('.sections.active').first().attr('id')
      let graphId = $('#' + val + 'graphs option:selected').val()
      if (graphId !== undefined) {
        this.loadGraph(val, graphId)
      }
      this.hide()
    },
    openView: function (view) {
      $('.graphviews').addClass('hiddenView')
      $('#' + view).removeClass('hiddenView')
    },
    FindGraphById: function (graph) {
      if (String(graph.graph_id) === String(this.graphId)) {
        return graph
      }
    },
    loadGraph: function (type, graphId) {
      console.log(graphId)
      this.graphId = graphId
      let graph = ''
      if (type === 'root') {
        graph = this.rootgraphs.find(this.FindGraphById, graphId)
      } else if (type === 'user') {
        graph = this.usergraphs.find(this.FindGraphById, graphId)
      }
      let graphData = JSON.parse(graph.content)
      this.cy.json(graphData)
      this.setCurrentGraphId(graphId)
      this.setCurrentGraphType(type)
      this.setCurrentGraphTitle(graph.title)
    }
  },
  mounted () {
    $('.sections').click(function () {
      $('.active').removeClass('active')
      $(this).addClass('active')
    })

    this.updateRootGraphs()
    this.updateUserGraphs()
  }
}
</script>

<style scoped>
  .hiddenView{
    display: none;
  }
</style>

