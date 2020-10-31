<template>
  <div>
    <base-header class="mb-6 pb-2 pt-3 pt-md-4 bg-default">
      <!-- Card stats -->
      <h4 class="text-light">
        Extract configuration for various cloud Identities from METADATA
      </h4>
    </base-header>
   
    <b-container fluid class="mt--7">
      <b-row>
        <!-- <base-alert type="danger" v-on="showSamlParseError" dismissible> {{samlParseErrorMessage}} </base-alert> -->
        <b-alert
          variant="danger"
          dismissible
          fade
          :show="showSamlParseError"
          @dismissed="showSamlParseError=false">
            {{samlParseErrorMessage}}
        </b-alert> 
      
      </b-row>
      <b-row> 
        <b-card-body>
          <form>
            <base-input label="Paste SAML metadata here">
              <textarea
                id="exampleFormControlTextarea3"
                v-model="samlText"
                class="form-control"
                rows="3"
              />
            </base-input>
            <div class="text-left">
              <base-button
                type="primary"
                native-type="submit"
                class="my-4"
                @click="parseXML(samlText)"
              >
                Process XML
              </base-button>
            </div>
          </form>
        </b-card-body>
      </b-row>
      <b-tabs content-class="mt-3" v-if="samlMetaData.metaType == 'IDP'">
        <b-tab title="Okta" active>
          <template #title>
            <b-img width="40" height="40" fluid src="https://www.okta.com/sites/default/files/Okta_Logo_BrightBlue_Medium-thumbnail.png" alt="Okta">Okta</b-img>
            <b-spinner type="grow" small></b-spinner> Okta</i>
          </template>
          <CopyBox
            :value="samlMetaData.IDPOptions.audienceUrl"
            label="Audience URL"
          />
          <CopyBox :value="samlMetaData.IDPOptions.postUrl" label="Post URL " />
          <CopyBox
            :value="samlMetaData.IDPOptions.redirectUrl"
            label="Redirect URL"
          />
          ></b-tab
        >
        <b-tab title="Ping ">
           <template #title>
            <b-img width="20" height="15" fluid src="https://www.pingidentity.com/content/dam/ping-6-2-assets/topnav-json-configs/Ping-Logo.svg" alt="Okta">Okta</b-img>
            <b-spinner type="grow" small></b-spinner> Ping</i>
          </template>
          <p>⌛Working on it , feel free to contribute..</p>
        </b-tab>
      </b-tabs>
    </b-container>
  </div>
</template>
<script>
import {
  Dropdown,
  DropdownItem,
  DropdownMenu,
  Table,
  TableColumn,
} from "element-ui";
import { CopyBox } from "../components/";
import { parseMetaData } from "../util/SAMLUtil";
export default {
  props: {},
  components: {
    [Dropdown.name]: Dropdown,
    [DropdownItem.name]: DropdownItem,
    [DropdownMenu.name]: DropdownMenu,
    [Table.name]: Table,
    [TableColumn.name]: TableColumn,
    CopyBox,
  },
  data() {
    return {
      samlMetaData: {},
      samlText: "",
      showSamlParseError: false,
      samlParseErrorMessage: "",
    };
  },
  errorCaptured(err, components, info) {
    console.log("Error in component", info);
    if (err.name == "SAMLMetaParseError") {
      this.showSamlParseError = true;
      this.samlParseErrorMessage = err.message;
    }
  },
  methods: {
    parseXML(samlText) {
      this.samlMetaData = parseMetaData(samlText);
    },
  },
};
</script>
<style>
.el-table.table-dark {
  background-color: #172b4d;
  color: #f8f9fe;
}

.el-table.table-dark th,
.el-table.table-dark tr {
  background-color: #172b4d;
}

.el-table.table-dark td,
.el-table.table-dark th.is-leaf {
  border-bottom: none;
}
</style>
