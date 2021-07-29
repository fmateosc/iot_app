<template>
  <div>
    <!-- DEVICES TABLE -->
    <div class="row">
      <card>
        <div slot="header">
          Devices

          <base-button
            type="primary"
            class="btn btn-secondary"
            style="float: right;"
            @click="abrirModal('device', 'registrar')"
          >
            <i class="fas fa-plus"></i>&nbsp;Add
          </base-button>
        </div>

        <el-table :data="devices">
          <el-table-column label="#" min-width="50" align="center">
            <div slot-scope="{ row, $index }">
              {{ $index + 1 }}
            </div>
          </el-table-column>

          <el-table-column prop="name" label="Name"></el-table-column>

          <el-table-column prop="dId" label="Device Id"></el-table-column>

          <el-table-column prop="dType" label="Device type"></el-table-column>

          <el-table-column
            prop="templateName"
            label="Template"
          ></el-table-column>

          <el-table-column label="Actions">
            <div slot-scope="{ row, $index }">
              <el-tooltip content="Database Saver">
                <i 
                  class="fas fa-database " 
                  :class="{'text-success' : row.saverRule, 'text-dark' : !row.saverRule}"
                  style="margin-right: 5px">                 
                </i>    
              </el-tooltip>
                
              <el-tooltip content="Saver Status Indicator">
                <base-switch 
                  @click="updateSaverRuleStatus($index)" :value="row.saverRule" 
                  type="primary" 
                  style="margin-top: 10px"
                  on-text="On" 
                  off-text="Off">
                </base-switch>
              </el-tooltip>

              <el-tooltip
                content="Delete"
                effect="light"
                :open-delay="300"
                placement="top"
              >
                <base-button
                  type="danger"
                  icon
                  size="sm"
                  class="btn-link"
                  @click="deleteDevice(row)"
                >
                  <i class="fas fa-trash"></i>
                </base-button>
              </el-tooltip>
            </div>
          </el-table-column>
        </el-table>
      </card>
    </div>

    <!--Inicio del modal agregar/actualizar-->
    <div
      class="modal fade"
      tabindex="-1"
      :class="{ mostrar: modal }"
      role="dialog"
      aria-labelledby="myModalLabel"
      style="display: none;"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-primary modal-lg" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h4 class="modal-title" v-text="tituloModal"></h4>
            <button
              type="button"
              class="close"
              @click="cerrarModal()"
              aria-label="Close"
            >
              <span aria-hidden="true">×</span>
            </button>
          </div>
          <div class="modal-body">
            <hr>
            <div class="form-group row">
              <label class="col-md-3 form-control-label" for="text-input"
                >Device name</label
              >
              <div class="col-md-9">
                <input
                  type="text"
                  style="color:black"
                  class="form-control"
                  placeholder="Ex: Home, Office..."
                />
              </div>
            </div>

            <div class="form-group row">
              <label class="col-md-3 form-control-label" for="text-input"
                >Device id</label
              >
              <div class="col-md-9">
                <input
                  type="text"
                  style="color:black"
                  class="form-control"
                  placeholder="Ex: 7777-8888"
                />
              </div>
            </div>
            <div class="form-group row">
              <label class="col-md-3 form-control-label" for="text-input"
                >Device type</label
              >
              <div class="col-md-9">
                <input
                  type="text"
                  style="color:black"
                  class="form-control"
                  placeholder="Ex: ESP32"
                />
              </div>
            </div>

            <div class="form-group row">
              <label class="col-md-3 form-control-label" for="text-input"
                >Template</label
              >
              <div class="col-md-9">
                <select 
                  class="form-control" 
                  style="color:black">
                  <option>Template 1</option>
                  <option>Template 2</option>
                  <option>Template 3</option>
                  <option>Template 4</option>
                  <option>Template 5</option>
                </select>
              </div>
            </div>
          </div>
          <div class="modal-footer">
            <base-button
              type="button"
              class="btn btn-secondary"
              @click="cerrarModal()"
            >
              Close
            </base-button>
            <base-button
              type="button"
              v-if="tipoAccion == 1"
              class="btn btn-primary"
            >
              Save
            </base-button>

            <base-button
              type="button"
              v-if="tipoAccion == 2"
              class="btn btn-primary"
            >
              Update
            </base-button>
          </div>
        </div>
        <!-- /.modal-content -->
      </div>
      <!-- /.modal-dialog -->
    </div>
    <!--Fin del modal-->

    <Json :value="devices"></Json>
  </div>
</template>

<script>
import { Table, TableColumn } from "element-ui";
import { Select, Option } from "element-ui";
export default {
  components: {
    [Table.name]: Table,
    [TableColumn.name]: TableColumn,
    [Option.name]: Option,
    [Select.name]: Select
  },
  data() {
    return {
      modal: 0,
      tituloModal: "",
      tipoAccion: 0,
      devices: [
        {
          name: "Home",
          dId: "8888",
          dType: "ESP 32",
          templateName: "Power Sensor",
          templateId: "984237562348756ldksjfh",
          saverRule: false
        },
        {
          name: "Office",
          dId: "1111",
          dType: "ESP 32",
          templateName: "Power Sensor",
          templateId: "984237562348756ldksjfh",
          saverRule: true
        },
        {
          name: "Farm",
          dId: "99999",
          dType: "ESP 32",
          templateName: "Power Sensor",
          templateId: "984237562348756ldksjfh",
          saverRule: false
        }
      ]
    };
  },
  methods: {
    deleteDevice(device) {
      alert("DELETING " + device.name);
    },
    updateSaverRuleStatus(index){
      console.log(index)
      this.devices[index].saverRule = !this.devices[index].saverRule;
    },
    abrirModal(modelo, accion, data = []) {
      switch (modelo) {
        case "device": {
          switch (accion) {
            case "registrar": {
              this.modal = 1;
              this.tituloModal = "Add device";
              this.tipoAccion = 1;

              break;
            }
            case "actualizar": {
              //console.log(data);
              this.modal = 1;
              this.tituloModal = "Update device";
              this.tipoAccion = 2;

              break;
            }
          }
        }
      }
    },
    cerrarModal(){
        this.modal=0;
        this.tituloModal='';
        this.nombre='';
        this.descripcion='';
    }
  }
};
</script>

<style>
.modal-content {
  margin-top: 100px;
  width: 100% !important;
  position: absolute !important;
}
.mostrar {
  display: list-item !important;
  opacity: 1 !important;
  position: absolute !important;
  background-color: #3c29297a !important;
}
</style>
