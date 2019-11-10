<template>
  <div class="wrapper" @keydown="keyShortCuts">
    <div id="fs_control">
      <div class="panel panel-default">
        <div class="headPanel panel-heading first-row">
          <div class="btn-group">
            <button
              id="submit"
              type="button"
              class="btn btn-sm btn-run"
              :class="{ loading : disabled }"
              @click="runCode()"
              :disabled="loading"
            >
              <i class="fa fa-play" aria-hidden="true"></i>
              <span v-if="loading">Running</span>
              <span v-else>Run</span>
            </button>
            <language :options="languages" :selected="this.$store.state.language"></language>

            <!-- Added div container for other elements for responsiveness  -->
            <div class="big-menu">
              <button class="btn btn-sm btn-menu">
                <router-link
                  class="decoration-none"
                  to="/"
                  target="_blank"
                  active-class
                  exact-active-class
                >
                  New
                  <i class="fa fa-file-code-o" aria-hidden="true"></i>
                </router-link>
              </button>

              <button
                type="button"
                id="custInp"
                class="btn btn-sm btn-menu"
                @click="InOutBoxToggle()"
              >
                Input
                <i class="fa fa-keyboard-o" aria-hidden="true"></i>
              </button>
              <button type="button" id="save" class="btn btn-sm btn-menu" @click="saveToServer()">
                Save
                <i class="fa fa-floppy-o" aria-hidden="true"></i>
              </button>
              <button
                type="button"
                id="download"
                class="btn btn-sm btn-menu"
                @click="showDownloadModal()"
              >
                Download
                <i class="fa fa-download" aria-hidden="true"></i>
              </button>
              <input type="file" ref="fileUpload" style="display:none" @change="uploadCode" />
              <button type="button" id="uploadFile" class="btn btn-sm btn-menu" @click="selectFile">
                Upload
                <span class="fa fa-folder-open" aria-hidden="true"></span>
              </button>
              <input type="file" id="upload" style="display:none;" />
              <button
                type="button"
                id="settingButton"
                class="btn btn-sm btn-menu"
                @click="settingsToggle"
              >
                Setting
                <span class="fa fa-cog"></span>
              </button>
              <share></share>
              <button
                id="panelLang"
                type="button"
                class="btn btn-sm btn-menu"
                @click="showShortcutsModal()"
              >
                Shortcuts
                <i class="fa fa-reply-all" aria-hidden="true"></i>
              </button>
            </div>
          </div>

          <div class="right-menu">
            <div style="display:none" class="ham-menu">
              <header class="header">
                <nav class="header__nav">
                  <ul class="header__navbar">
                    <li class="header__item">
                      <a href="#" class="header__link">
                        <transition name="slide-fade">
                          <!-- Header Navigation Menu Icons -->
                          <button class="header--button" v-if="show" key="on" @click="show = false">
                            <svg viewBox="0 0 24 24" class="header--icon">
                              <title>Close</title>
                              <path d="M0 0h24v24H0V0z" fill="none" />
                              <path
                                fill="currentColor"
                                d="M18.3 5.71c-.39-.39-1.02-.39-1.41 0L12 10.59 7.11 5.7c-.39-.39-1.02-.39-1.41 0-.39.39-.39 1.02 0 1.41L10.59 12 5.7 16.89c-.39.39-.39 1.02 0 1.41.39.39 1.02.39 1.41 0L12 13.41l4.89 4.89c.39.39 1.02.39 1.41 0 .39-.39.39-1.02 0-1.41L13.41 12l4.89-4.89c.38-.38.38-1.02 0-1.4z"
                              />
                            </svg>
                          </button>
                          <button class="header--button" v-else key="off" @click="show = true">
                            <svg viewBox="0 0 24 24" class="header--icon">
                              <title>Navigation Menu</title>
                              <path
                                fill="currentColor"
                                d="M6,8c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2zM12,20c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2zM6,20c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2zM6,14c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2zM12,14c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2zM16,6c0,1.1 0.9,2 2,2s2,-0.9 2,-2 -0.9,-2 -2,-2 -2,0.9 -2,2zM12,8c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2zM18,14c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2zM18,20c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2z"
                              />
                            </svg>
                          </button>
                        </transition>
                      </a>
                      <!-- Dropdown Menu -->
                      <transition name="dropdown">
                        <div class="dropdown__menu" v-bind:class="{ active: show }" v-if="show">
                          <ul class="dropdown__menu-nav">
                            <li class="dropdown__menu-item">
                              <a href="#" class="dropdown__menu-link" title="Account">
                                <button class="btn btn-sm btn-menu">
                                 <i class="fa fa-file-code-o" aria-hidden="true"></i>
                                  <router-link
                                    class="decoration-none"
                                    to="/"
                                    target="_blank"
                                    active-class
                                    exact-active-class
                                  >
                                    New
                                  </router-link>
                                </button>
                              </a>
                            </li>
                            <li class="dropdown__menu-item">
                              <a href="#" class="dropdown__menu-link" title="Personal info">
                                <button
                                  type="button"
                                  id="custInp"
                                  class="btn btn-sm btn-menu"
                                  @click="InOutBoxToggle()"
                                >
                                  <i class="fa fa-keyboard-o" aria-hidden="true"></i>
                                  Input
                                </button>
                              </a>
                            </li>
                            <li class="dropdown__menu-item">
                              <a href="#" class="dropdown__menu-link" title="Privacy">
                                <button
                                  type="button"
                                  id="save"
                                  class="btn btn-sm btn-menu"
                                  @click="saveToServer()"
                                >
                                <i class="fa fa-floppy-o" aria-hidden="true"></i>
                                  Save
                                </button>
                              </a>
                            </li>
                            <li class="dropdown__menu-item">
                              <a
                                href="#"
                                class="dropdown__menu-link"
                                title="Data &amp; personalization"
                              >
                                <button
                                  type="button"
                                  id="download"
                                  class="btn btn-sm btn-menu"
                                  @click="showDownloadModal()"
                                >
                                <i class="fa fa-download" aria-hidden="true"></i>
                                  Download
                                </button>
                                <input
                                  type="file"
                                  ref="fileUpload"
                                  style="display:none"
                                  @change="uploadCode"
                                />
                              </a>
                            </li>
                            <li class="dropdown__menu-item">
                              <a href="#" class="dropdown__menu-link" title="People &amp; sharing">
                                <button
                                  type="button"
                                  id="uploadFile"
                                  class="btn btn-sm btn-menu"
                                  @click="selectFile"
                                >
                                <span class="fa fa-folder-open" aria-hidden="true"></span>
                                  Upload
                                </button>
                                <input type="file" id="upload" style="display:none;" />
                              </a>
                            </li>
                            <li class="dropdown__menu-item">
                              <a href="#" class="dropdown__menu-link" title="Subscriptions">
                                <button
                                  type="button"
                                  id="settingButton"
                                  class="btn btn-sm btn-menu"
                                  @click="settingsToggle"
                                >
                                  <span class="fa fa-cog"></span>
                                  Setting
                                </button>
                              </a>
                            </li>
                            <li class="dropdown__menu-item">
                              <a href="#" class="dropdown__menu-link">
                                  <share></share>
                              </a>
                            </li>
                            <li class="dropdown__menu-item">
                              <a href="#" class="dropdown__menu-link" title="Subscriptions">
                                <button
                                  id="panelLang"
                                  type="button"
                                  class="btn btn-sm btn-menu"
                                  @click="showShortcutsModal()"
                                >
                                <i class="fa fa-keyboard-o" aria-hidden="true"></i>
                                  Shortcuts
                                </button>
                              </a>
                            </li>
                            <!-- Dropdown Menu Separator -->
                            <li  style="display:none" class="dropdown__menu-item auth-ham">
                                 <hr />
                                <div class="hamLogoMenu">
                                  <login-button></login-button>Made with
                                  <i
                                    class="fa fa-heart"
                                    aria-hidden="true"
                                    style="color: #e31d3b"
                                  ></i> by
                                  <img src="../../assets/cb_logo_light.png" />
                                </div>
                            </li>
                          </ul>
                        </div>
                      </transition>
                    </li>
                  </ul>
                </nav>
              </header>
            </div>
            <div class="logoMenu">
              <login-button></login-button>Made with
              <i class="fa fa-heart" aria-hidden="true" style="color: #e31d3b"></i> by
              <img src="../../assets/cb_logo_light.png" />
            </div>
          </div>
        </div>
        <div class="panel-heading second-row">
          Title:
          <input
            class="black"
            type="text"
            placeholder="Untitled"
            :value="this.$store.state.codeTitle"
            @change="changeTitle"
          />
        </div>
      </div>
      <settings v-show="this.$store.state.showSettings"></settings>
    </div>

    <modal name="download-modal" transition="pop-out" :width="680" :pivot-y="0.2" :height="auto">
      <div class="download-modal-title flex-center">confirm file name</div>
      <div class="download-modal-content flex-center">
        <span>File Name:</span>
        <input
          v-on:keyup.enter="downloadCode"
          v-on:change="updateFileName"
          ref="fileName"
          :value="this.$store.state.fileName"
          placeholder="Enter File name"
        />
      </div>
      <div class="download-modal-button-set flex-space-between">
        <button class="modal-button" @click="closeDownloadModal()">close</button>
        <button class="modal-button" @click="resetFileName()">reset file name</button>
        <button class="modal-button" @click="saveFileName()">save file name</button>
        <button class="modal-button" @click="downloadCode()">download code</button>
      </div>
    </modal>

    <modal name="shortcuts-modal" transition="pop-out" :width="600" :pivot-y="0.3" :height="500">
      <div class="shortcuts-modal-title flex-center">
        keyboard shortcuts
        <span @click="closeShortcutsModal()" class="shortcuts-modal-close">×</span>
      </div>
      <ul class="shortcuts-modal-content">
        <li class="key-unit flex-space-between">
          <span class="key-span flex-center">Ctrl + I</span>
          <span class="key-description">To run the program in windows/linux</span>
        </li>
        <li class="key-unit flex-space-between">
          <span class="key-span flex-center">⌘ + I</span>
          <span class="key-description">To run the program in mac</span>
        </li>
        <li class="key-unit flex-space-between">
          <span class="key-span flex-center">Ctrl + U</span>
          <span class="key-description">To reset the settings in windows/linux</span>
        </li>
        <li class="key-unit flex-space-between">
          <span class="key-span flex-center">⌘ + U</span>
          <span class="key-description">To reset the settings in mac</span>
        </li>
        <li class="key-unit flex-space-between">
          <span class="key-span flex-center">Ctrl + B</span>
          <span class="key-description">To reset the code in windows/linux</span>
        </li>
        <li class="key-unit flex-space-between">
          <span class="key-span flex-center">⌘ + B</span>
          <span class="key-description">To reset the code in mac</span>
        </li>
        <li class="key-unit flex-space-between">
          <span class="key-span flex-center">Ctrl + S</span>
          <span class="key-description">To download the code in windows/linux</span>
        </li>
        <li class="key-unit flex-space-between">
          <span class="key-span flex-center">⌘ + S</span>
          <span class="key-description">To download the code in mac</span>
        </li>
      </ul>
    </modal>
  </div>
</template>

<script>
import language from "./Language.vue";
import Vue from "vue";
import base64 from "base-64";
import Settings from "./Settings.vue";
import Share from "./Share.vue";
import LoginButton from "../auth/LoginButton.vue";
import * as download from "downloadjs";
export default {
  name: "menuBar",
  components: { language, Settings, Share, LoginButton },
  data() {
    return {
      languages: [
        "C",
        "C++",
        "C#",
        "Java",
        "Python",
        "Python3",
        "Javascript",
        "NodeJs",
        "Ruby"
      ],
      show: false,
      fullscreen: false,
      loading: false,
      fileName: this.$store.state.fileName
    };
  },
  watch: {
      show() {
          if(this.$store.state.showSettings) {
               this.$store.state.showSettings = !this.$store.state.showSettings
          }
      }
  },
  methods: {
    runCode() {
      this.loading = !this.loading;
      this.$store
        .dispatch("runCode")
        .then(data => {
          if (!this.$store.state.showInOutBox)
            this.$store.commit("toggleInOutBox");
          this.loading = false;
          if (data.result == "compile_error") {
            this.$notify({
              text: "Compilation Error",
              type: "error"
            });
          } else if (data.result == "success") {
            if (data.data.testcases[0].result == "run-error") {
              this.$notify({
                text: "Runtime Error",
                type: "error"
              });
            } else {
              this.$notify({
                text: "Code Compiled Successfully",
                type: "success"
              });
            }
          }
        })
        .catch(err => {
          console.error(err);
          this.loading = false;
          this.$notify({
            text:
              "There was some error while running the code, please try again.",
            type: "error"
          });
        });
    },
    saveToServer() {
      this.$store.dispatch("saveDataToServer").then(({ data }) => {
        this.$notify({
          text: "Code saved to server",
          type: "success"
        });
        return this.$router.push({ name: "saved", params: { id: data.id } });
      });
    },
    InOutBoxToggle() {
      this.$store.commit("toggleInOutBox");
    },
    settingsToggle() {
       this.$store.commit("toogleSettings");
    },
    selectFile() {
      // open file select dialogue
      this.$refs.fileUpload.click();
    },
    showDownloadModal() {
      this.$modal.show("download-modal");
      setTimeout(() => {
        this.$refs.fileName.select();
      }, 200);
    },
    closeDownloadModal() {
      this.$modal.hide("download-modal");
      this.$data.fileName = this.$store.state.fileName;
    },
    updateFileName(e) {
      e.preventDefault();
      this.$data.fileName = e.target.value;
    },
    resetFileName() {
      this.$store.commit("changeLanguage", this.$store.state.language);
      this.$refs.fileName.value = this.$data.fileName = this.$store.state.fileName;
      this.$refs.fileName.select();
    },
    saveFileName() {
      this.$store.commit("fileNameChange", this.$data.fileName);
      this.$modal.hide("download-modal");
    },
    downloadCode() {
      this.saveFileName();
      const code = this.$store.state.code[this.$store.state.language];
      download(
        `data:text/plain;charset=utf-8,${encodeURIComponent(code)}`,
        this.$store.state.fileName,
        "text/plain"
      );
    },
    uploadCode(e) {
      const files = e.target.files || e.dataTransfer.files;
      if (!files.length) {
        return;
      }

      const file = files[0];
      const reader = new FileReader();

      reader.onload = e => {
        console.log("Uploaded File: " + file.name);
        this.$notify({
          text: "Code Uploaded Successfully",
          type: "success"
        });
        this.$store.commit("uploadCode", e.target.result);
        this.$store.commit("fileNameChange", file.name);
        this.$refs.fileUpload.value = "";
      };
      reader.readAsText(file);
    },
    showShortcutsModal() {
      this.$modal.show("shortcuts-modal");
    },
    closeShortcutsModal() {
      this.$modal.hide("shortcuts-modal");
    },
    keyShortCuts(e) {
      const isMacLike = navigator.platform.match(/(Mac|iPad)/i) ? true : false;
      const isMetaOrCtrlDown = (isMacLike && e.metaKey) || e.ctrlKey;
      if (isMetaOrCtrlDown && e.keyCode === 73) {
        e.preventDefault();
        this.runCode();
      }
      if (isMetaOrCtrlDown && e.keyCode === 85) {
        e.preventDefault();
        this.$store.commit("resetEditor");
      }
      if (isMetaOrCtrlDown && e.keyCode === 83) {
        e.preventDefault();
        this.showDownloadModal();
      }
      if (isMetaOrCtrlDown && e.keyCode === 66) {
        e.preventDefault();
        this.$store.commit("resetCode", this.$store.state.language);
      }
    },
    changeTitle(e) {
      this.$store.commit("setCodeTitle", e.target.value);
    }
  }
};
</script>

<style scoped>
#fs_control {
  position: relative;
  z-index: 20;
}
#fs_control .headPanel {
    height: auto !important;
}
.panel {
  width: 100vw;
  height: 40px;
  margin: 0;
  border: none;
  border-bottom: 1px;
}

.panel-heading {
  padding: 0 15px;
  background: #202020;
  color: #fff;
  border-color: #272727;
}

.first-row {
  display: flex;
  justify-content: space-between;
}

.logoMenu {
  float: right;
  font-weight: 700;
  text-transform: uppercase;
}



.logoMenu img , .hamLogoMenu img{
  height: 35px;
}

@media (max-width: 677px) {
  .logoMenu {
    display: none;
  }
  .auth-ham {
      display:block !important;
  }
}

@media (max-width: 1248px) {
    .big-menu {
        display: none;
    }
    .ham-menu {
        display:initial !important;
    }
} 

</style>

<style>
.btn-run {
  background: #e31d3b !important;
  border-radius: 50px !important;
  color: white !important;
}

.btn-run:hover,
.btn-run:focus,
.btn-run:active {
  box-shadow: none !important;
}

.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
  color: black;
}

.modal-container {
  width: 45vw;
  margin: 0px auto;
  padding: 10px 30px 30px 30px;
  background-color: #fff;
  border-radius: 3px !important;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header {
  padding: 15px 25px !important;
}

.modal-footer {
  padding: 15px 0 0 0;
}

.modal-header h3 {
  margin-top: 0;
  margin-bottom: 0;
  font-weight: 400;
}

.modal-body {
  margin: 5px 0;
  font-size: 15px;
}

.modal-default-button {
  float: right;
}

.modal-footer button {
  color: white;
}

.modal-body ul li kbd {
  display: inline-block;
  margin: 0 0.1em;
  padding: 0.2em 1em;
  font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
  font-size: 10px;
  line-height: 1.4;
  color: #242729;
  text-shadow: 0 1px 0 #fff;
  background-color: #e1e3e5;
  border: 1px solid #adb3b9;
  border-radius: 3px;
  box-shadow: 0 1px 0 rgba(12, 13, 14, 0.2), 0 0 0 2px #fff inset;
}

.flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

.flex-space-between {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.download-modal-title {
  height: 60px;
  font-size: 24px;
  font-weight: 500;
}

.download-modal-content,
.download-modal-title,
.download-modal-button-set {
  letter-spacing: 1px;
  padding: 8px;
  text-transform: uppercase;
}

.download-modal-button-set .modal-button {
  font-size: 16px;
  text-transform: uppercase;
  color: #8b8c8d;
  background: white;
  border-radius: 4px;
  box-sizing: border-box;
  padding: 10px;
  min-width: 100px;
  margin: 4px;
  cursor: pointer;
  border: 1px solid #b4b4b4;
  transition: 0.1s all;
  outline: none;
}

.download-modal-button-set .modal-button:hover {
  border: 1px solid #181818;
  color: #181818;
}

.download-modal-button-set {
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 80px;
}

.download-modal-title {
  border-bottom: 2px solid #ccc;
}

.download-modal-content {
  height: calc(100% - 140px);
}

.download-modal-content input {
  display: block;
  box-sizing: border-box;
  margin-bottom: 4px;
  padding: 8px;
  margin: 0 8px;
  width: 280px;
  font-size: 16px;
  border: 0;
  border-bottom: 1px solid #b4b4b4;
  color: #b4b4b4;
  font-family: inherit;
  transition: 0.5s all;
  outline: none;
}

.download-modal-content input:focus {
  border-bottom: 1px solid #181818;
  color: #181818;
}

.shortcuts-modal-title,
.shortcuts-modal-content {
  padding: 8px;
  color: #b4b4b4;
}

.shortcuts-modal-content {
  height: calc(100% - 60px);
  overflow-y: auto;
}

.shortcuts-modal-title {
  font-size: 24px;
  font-weight: 500;
  text-transform: uppercase;
  height: 60px;
  border-bottom: 2px solid #ccc;
}

.shortcuts-modal-content {
  font-size: 16px;
  font-weight: 400;
}

.key-span {
  color: #555;
  text-align: center;
  background-color: #eee;
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #ccc;
  box-shadow: inset 0 1px 0 #fff, 0 1px 0 #ccc;
  font-size: 20px;
  padding: 4px 8px;
  margin: 0 8px;
}

.key-unit {
  text-transform: uppercase;
  width: calc(100% - 16px);
  margin: 8px;
}

.shortcuts-modal-close {
  position: absolute;
  right: 15px;
  top: 15px;
  font-size: 40px;
  font-weight: 500;
  cursor: pointer;
}

.shortcuts-modal-close:hover {
  color: #181818;
}

.decoration-none {
  color: unset;
}
.decoration-none:hover {
  color: #fc4f4f;
}
.second-row {
  padding: 0px 25px !important;
  font-family: sans-serif;
  font-weight: 600;
  font-size: 1.05rem;
}
.second-row input {
  margin-bottom: 5px;
}

/* menu bar css */

.right-menu {
  display: flex;
  justify-content: space-between;
}
.header {
  /* padding: 1rem 5rem 2rem 5rem; */
    padding: 0.3rem 1rem;
}
.header__nav {
  position: relative;
  background: transparent !important;
}
.header__navbar {
  display: flex;
  align-items: center;
  justify-content: flex-end;
}
.header__item {
  padding: 1rem;
}
.header--icon {
  width: 1.65rem;
  height: 1.65rem;
}
.header--button {
  top: 0;
  right: 0;
  position: absolute;
  margin: 0;
  padding: 0;
  color: gray;
  cursor: pointer;
  border: 1px solid transparent;
  background-color: transparent;
}
.header--button:focus {
  outline: 0;
}
.dropdown__menu {
  top: 100%;
  right: 0;
  position: absolute;
  z-index: 10;
  min-width: 300px;
  margin-top: 1rem;
  overflow-y: auto;
  padding: 2rem 1rem 2rem 0rem;
  border-radius: 12px;
  background-color: #202020;
  box-shadow: 0 2px 6px 2px #101010;
  border: 1px solid var(--color-gray);
  background-clip: padding-box;
}
.dropdown__menu-link {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  text-decoration: none;
  color: rgba(0, 0, 0, 0.6);
  /* padding: 0.8rem 0 0.8rem 2rem; */
  padding:0.2rem;
  margin-top: 0.2rem;
  margin-bottom: 0.2rem;
  border-radius: 0 50px 50px 0;
}
.dropdown__menu-link:hover {
  color: #fc4f4f !important;
  /* background-color: rgba(79, 192, 141, 0.1); */
  background-color: #292828;
}
.dropdown__menu-svg {
  width: 1.5rem;
  height: 1.5rem;
}
.dropdown__menu-text {
  font-weight: 300;
  margin-left: 1rem;
  margin-right: 1rem;
}
.slide-fade-enter-active,
.slide-fade-leave-active {
  transition: all 0.6s;
}
.slide-fade-enter,
.slide-fade-leave-active {
  opacity: 0;
}
.slide-fade-enter {
  transform: translateX(31px);
}
.slide-fade-leave-active {
  transform: translateX(-31px);
}
.dropdown-enter-active,
.dropdown-leave-active {
  transition: all 1s;
}
.dropdown-enter,
.dropdown-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
</style>
