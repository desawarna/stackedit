<template>
  <div class="side-bar__panel side-bar__panel--menu">
    <div class="workspace" v-for="(workspace, id) in workspacesById" :key="id">
      <menu-entry :href="workspace.url" target="_blank">
        <icon-provider slot="icon" :provider-id="workspace.providerId"></icon-provider>
        <div class="workspace__name"><div class="menu-entry__label" v-if="currentWorkspace === workspace">current</div>{{workspace.name}}</div>
      </menu-entry>
    </div>
    <hr>
    <menu-entry @click.native="addCouchdbWorkspace">
      <icon-provider slot="icon" provider-id="couchdbWorkspace"></icon-provider>
      <div>CouchDB workspace</div>
      <span>Add a workspace synced with a CouchDB database.</span>
    </menu-entry>
    <menu-entry @click.native="addGithubWorkspace">
      <icon-provider slot="icon" provider-id="githubWorkspace"></icon-provider>
      <div>GitHub workspace</div>
      <span>Add a workspace synced with a GitHub repository.</span>
    </menu-entry>
    <menu-entry @click.native="addGoogleDriveWorkspace">
      <icon-provider slot="icon" provider-id="googleDriveWorkspace"></icon-provider>
      <div>Google Drive workspace</div>
      <span>Add a workspace synced with a Google Drive folder.</span>
    </menu-entry>
    <menu-entry @click.native="manageWorkspaces">
      <icon-database slot="icon"></icon-database>
      <span><div class="menu-entry__label menu-entry__label--count">{{workspaceCount}}</div> Manage workspaces</span>
    </menu-entry>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
import MenuEntry from './common/MenuEntry';
import googleHelper from '../../services/providers/helpers/googleHelper';

export default {
  components: {
    MenuEntry,
  },
  computed: {
    ...mapGetters('workspace', [
      'workspacesById',
      'currentWorkspace',
    ]),
    workspaceCount() {
      return Object.keys(this.workspacesById).length;
    },
  },
  methods: {
    async addCouchdbWorkspace() {
      try {
        this.$store.dispatch('modal/open', {
          type: 'couchdbWorkspace',
        });
      } catch (e) {
        // Cancel
      }
    },
    async addGithubWorkspace() {
      try {
        this.$store.dispatch('modal/open', {
          type: 'githubWorkspace',
        });
      } catch (e) {
        // Cancel
      }
    },
    async addGoogleDriveWorkspace() {
      try {
        const token = await googleHelper.addDriveAccount(true);
        this.$store.dispatch('modal/open', {
          type: 'googleDriveWorkspace',
          token,
        });
      } catch (e) {
        // Cancel
      }
    },
    manageWorkspaces() {
      this.$store.dispatch('modal/open', 'workspaceManagement');
    },
  },
};
</script>

<style lang="scss">
@import '../../styles/variables.scss';

.workspace .menu-entry {
  padding-top: 12px;
  padding-bottom: 12px;
}

.workspace__name {
  font-weight: bold;
  line-height: 1.2;
}
</style>
