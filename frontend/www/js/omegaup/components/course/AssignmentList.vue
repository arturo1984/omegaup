<template>
  <div class="omegaup-course-assignmentlist panel">
    <div class="panel-heading">
      <h3>{{ T.wordsAssignments }}</h3>
    </div>
    <div class="panel-body"
         v-if="assignments.length == 0">
      <div class="empty-category">
        {{ T.courseAssignmentEmpty }}
      </div>
    </div>
    <div class="panel-body"
         v-else="">
      <table class="table table-striped">
        <thead>
          <tr>
            <th colspan="4"
                v-text="T.wordsHomeworks"></th>
          </tr>
        </thead>
        <tbody v-sortable="{ onUpdate: sortHomeworks }">
          <tr v-bind:key="assignment.alias"
              v-for="assignment in homeworks">
            <td>
              <a v-bind:title="T.courseAssignmentReorder"><span aria-hidden="true"
                    class="glyphicon glyphicon-move handle"></span></a>
            </td>
            <td>
              <a v-bind:href="assignmentUrl(assignment)">{{ assignment.name }}</a>
            </td>
            <td class="button-column">
              <a v-bind:title="T.courseAssignmentEdit"
                  v-on:click="onEdit(assignment)"><span aria-hidden="true"
                    class="glyphicon glyphicon-edit"></span></a>
            </td>
            <td class="button-column">
              <a v-bind:title="T.courseAssignmentDelete"
                  v-on:click="onDelete(assignment)"><span aria-hidden="true"
                    class="glyphicon glyphicon-remove"></span></a>
            </td>
          </tr>
        </tbody>
      </table>
      <hr>
      <table class="table table-striped">
        <thead>
          <tr>
            <th colspan="4"
                v-text="T.wordsTests"></th>
          </tr>
        </thead>
        <tbody v-sortable="{ onUpdate: sortTests }">
          <tr v-bind:key="assignment.alias"
              v-for="assignment in tests">
            <td>
              <a v-bind:title="T.courseAssignmentReorder"><span aria-hidden="true"
                    class="glyphicon glyphicon-move handle"></span></a>
            </td>
            <td>
              <a v-bind:href="assignmentUrl(assignment)">{{ assignment.name }}</a>
            </td>
            <td class="button-column">
              <a v-bind:title="T.courseAssignmentEdit"
                  v-on:click="onEdit(assignment)"><span aria-hidden="true"
                    class="glyphicon glyphicon-edit"></span></a>
            </td>
            <td class="button-column">
              <a v-bind:title="T.courseAssignmentDelete"
                  v-on:click="onDelete(assignment)"><span aria-hidden="true"
                    class="glyphicon glyphicon-remove"></span></a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="panel-footer">
      <form class="new">
        <div class="row">
          <div class="form-group col-md-12">
            <div class="pull-right">
              <button class="btn btn-primary"
                   type="submit"
                   v-on:click.prevent="onNew">{{ T.courseAssignmentNew }}</button>
            </div>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    T: Object,
    assignments: Array,
    courseAlias: String,
  },
  data: function() { return {};},
  computed: {
    homeworks: function() {
      return this.assignments.filter(
          (assignment) => { return assignment.assignment_type == 'homework'; });
    },
    tests: function() {
      return this.assignments.filter(
          (assignment) => { return assignment.assignment_type == 'test'; });
    }
  },
  methods: {
    assignmentUrl: function(assignment) {
      return '/course/' + this.courseAlias + '/assignment/' + assignment.alias +
             '/';
    },
    onDelete: function(assignment) { this.$emit('delete', assignment);},
    onEdit: function(assignment) { this.$emit('edit', assignment);},
    onNew: function() { this.$emit('new');},
    sortHomeworks: function(event) {
      this.homeworks.splice(event.newIndex, 0,
                            this.homeworks.splice(event.oldIndex, 1)[0]);
      this.$emit('sort-homeworks', this.courseAlias, this.homeworks);
    },
    sortTests: function(event) {
      this.tests.splice(event.newIndex, 0,
                        this.tests.splice(event.oldIndex, 1)[0]);
      this.$emit('sort-tests', this.courseAlias, this.tests);
    },
  },
};
</script>
