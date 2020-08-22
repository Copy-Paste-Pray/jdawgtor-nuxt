<template>
    <div>
        <div id="structure-gen">
            <div class="page-header">
                <h1 class="vert">Minecraft Structure Generator (no MSG!)</h1>
                <img class="logo" width="100" src="../assets/img/sg-logo-inverted.png" />
            </div>
            <section>
                <div>
                    <h2 class="section-title">Minecraft Version:</h2>
                    <div class="flex">
                        <select style="height: 28px;" name="mc-version" id="mc-version" v-model="sgver" :disabled="sgverEnabled==false">
                            <option value="null">--Select Version--</option>
                            <option value="1.3.0">1.16.0 - 1.16.1</option>
                            <option value="1.3.1">1.16.2</option>
                        </select>
                        <div class="bitpack" style="">
                            <span style="margin-bottom: 5px; display: inline-block;">Check out Bitpack by <a class="link" href="https://www.planetminecraft.com/member/16bitmap/" target="_blank">16bitmap</a></span>
                            <a class="btn" :href="'http://'+hostURL+'/sg/Bitpack-Custom-Structures-1.16.zip'" target="_blank">Download Bitpack</a>
                            <div class="notice">Bitpack works with Structure Generator (download below)</div>
                        </div>
                    </div>
                </div>
                
            </section>
            <section v-if="sgver">
                <h2 class="section-title">Upload Structure Files</h2>
                <form enctype="multipart/form-data" novalidate>
                    <div class="dropbox">
                    <input type="file" multiple :name="uploadFieldName" :disabled="isSaving" @change="filesChange($event.target.name, $event.target.files); fileCount = $event.target.files.length"
                        accept="image/*" class="input-file">
                        <p>
                        Drag your file(s) here<br> or click to browse
                        </p>
                        <p v-if="isSaving">
                        Uploading {{ fileCount }} files...
                        </p>
                    </div>
                </form>
            </section>
            <section v-if="msgOptions.length > 0">
                <div class="options" v-for="(fileUpload,index) in msgOptions" :key="fileUpload.filename">
                    <h2 class="file-title">
                        <span class="title1 red-bg">{{fileUpload.filename}} Options</span>
                        <span class="title2 green-bg"></span>
                        <span class="title3 blue-bg"></span>
                    </h2>
                    <div class="cols">
                        <div class="col">
                            <h2 class="section-title">Placement</h2>
                            <div class="notice">Where should structures generate?</div>
                            <select name="struct-type" id="struct-type" v-model="msgOptions[index].placement">
                                <option value="null">--Select Placement--</option>
                                <option value="surface">Surface</option>
                                <option value="caves">In Caves</option>
                                <option value="floating_islands">Floating Islands</option>
                                <option value="underwater">Underwater</option>
                                <option value="water">In Water</option>
                                <option value="lava">In Lava</option>
                                <option value="underlava">Under Lava</option>
                                <option value="y_range">Y Range</option>
                            </select>
                            <div class="notice">Default: <b>surface</b></div>
                            <div style="width: 100%;" v-if="msgOptions[index].placement == 'y_range'">
                                <div class="notice">Spawn structures at specific Y levels (i.e. 40 to 70)</div>
                                YRange&nbsp;Min: <input type="text" class="short" v-model="msgOptions[index].yRangeMin" placeholder="0"><br />
                                YRange&nbsp;Max: <input type="text" class="short" v-model="msgOptions[index].yRangeMax" placeholder="0">
                                <div class="notice">NOTE: Values 0 to 255 are valid.</div>
                            </div> 
                        </div>
                        <div class="col">
                            <h2 class="section-title">Rotation</h2>
                            <div class="notice">Allow structures to face different directions?</div>
                            <select name="rotation" id="rotation" v-model="msgOptions[index].rotation">
                                <option value="null">--Select Yes/No--</option>
                                <option value="yes">Yes</option>
                                <option value="no">No</option>
                            </select>
                            <div class="notice">Default: <b>yes</b></div>
                        </div>
                        <div class="col">
                            <h2 class="section-title">Rarity</h2>
                            <div class="notice">How often should structures spawn?</div>
                            <div class="slidecontainer">
                                <input type="range" min="1" max="100" value="50" class="slider" v-model="msgOptions[index].weight">
                                <span class="progress" v-if="msgOptions[index].weight">{{msgOptions[index].weight}}%</span>
                            </div>
                            <div class="notice">Default: <b>100%</b></div>
                        </div>
                    </div>
                    <div class="cols">
                        <div class="col">
                            <h2 class="section-title">Structure Block Position</h2>
                            <label for="">PosX:&nbsp;</label><input type="text" class="short" placeholder="posX" v-model="msgOptions[index].posx" />&nbsp;
                            <label for="">PosY:&nbsp;</label><input type="text" class="short" placeholder="posY" v-model="msgOptions[index].posy" />&nbsp;
                            <label for="">PosZ:&nbsp;</label><input type="text" class="short" placeholder="posZ" v-model="msgOptions[index].posz" />&nbsp;
                            <span class="notice">NOTE: Must be a number (ex. -123 or 123) </span>
                        </div>
                    </div>
                    <div class="cols">
                        <div class="col">
                            <h2 class="section-title">Dimension</h2>
                            <select v-model="msgOptions[index].dimension">
                                <option value="null">--Select Dimension--</option>
                                <option value="custom">Custom</option>
                                <option value="overworld">Overworld</option>
                                <option value="the_nether">The Nether</option>
                                <option value="the_end">The End</option>
                                
                            </select><br />
                            <div v-if="msgOptions[index].dimension=='custom'">
                                Custom Dimension:<br /><input type="text" v-model="msgOptions[index].dimensionOther" placeholder="i.e. mypack:mydimension" />
                            </div>
                        </div>
                        <div class="col">
                            <h2 class="section-title">Biome</h2>
                            <select v-model="msgOptions[index].biome">
                                <option value="null">--Select Biome--</option>
                                <option value="custom">Custom</option>
                                <option value="minecraft:badlands" data-i18n="badlands">Badlands</option>
                                <option value="minecraft:badlands_plateau" data-i18n="badlands_plateau">Badlands Plateau</option>
                                <option value="minecraft:bamboo_jungle" data-i18n="bamboo_jungle">Bamboo Jungle</option>
                                <option value="minecraft:bamboo_jungle_hills" data-i18n="bamboo_jungle_hills">Bamboo Jungle Hills</option>
                                <option value="minecraft:basalt_deltas" data-i18n="basalt_deltas">Basalt Deltas</option>
                                <option value="minecraft:beach" data-i18n="beach">Beach</option>
                                <option value="minecraft:birch_forest" data-i18n="birch_forest">Birch Forest</option>
                                <option value="minecraft:birch_forest_hills" data-i18n="birch_forest_hills">Birch Forest Hills</option>
                                <option value="minecraft:cold_ocean" data-i18n="cold_ocean">Cold Ocean</option>
                                <option value="minecraft:crimson_forest" data-i18n="crimson_forest">Crimson Forest</option>
                                <option value="minecraft:dark_forest" data-i18n="dark_forest">Dark Forest</option>
                                <option value="minecraft:dark_forest_hills" data-i18n="dark_forest_hills">Dark Forest Hills</option>
                                <option value="minecraft:deep_cold_ocean" data-i18n="deep_cold_ocean">Deep Cold Ocean</option>
                                <option value="minecraft:deep_frozen_ocean" data-i18n="deep_frozen_ocean">Deep Frozen Ocean</option>
                                <option value="minecraft:deep_lukewarm_ocean" data-i18n="deep_lukewarm_ocean">Deep Lukewarm Ocean</option>
                                <option value="minecraft:deep_ocean" data-i18n="deep_ocean">Deep Ocean</option>
                                <option value="minecraft:deep_warm_ocean" data-i18n="deep_warm_ocean">Deep Warm Ocean</option>
                                <option value="minecraft:desert" data-i18n="desert">Desert</option>
                                <option value="minecraft:desert_hills" data-i18n="desert_hills">Desert Hills</option>
                                <option value="minecraft:desert_lakes" data-i18n="desert_lakes">Desert Lakes</option>
                                <option value="minecraft:end_barrens" data-i18n="end_barrens">End Barrens</option>
                                <option value="minecraft:end_highlands" data-i18n="end_highlands">End Highlands</option>
                                <option value="minecraft:end_midlands" data-i18n="end_midlands">End Midlands</option>
                                <option value="minecraft:eroded_badlands" data-i18n="eroded_badlands">Eroded Badlands</option>
                                <option value="minecraft:flower_forest" data-i18n="flower_forest">Flower Forest</option>
                                <option value="minecraft:forest" data-i18n="forest">Forest</option>
                                <option value="minecraft:frozen_ocean" data-i18n="frozen_ocean">Frozen Ocean</option>
                                <option value="minecraft:frozen_river" data-i18n="frozen_river">Frozen River</option>
                                <option value="minecraft:giant_spruce_taiga" data-i18n="giant_spruce_taiga">Giant Spruce Taiga</option>
                                <option value="minecraft:giant_spruce_taiga_hills" data-i18n="giant_spruce_taiga_hills">Giant Spruce Taiga Hills</option>
                                <option value="minecraft:giant_tree_taiga" data-i18n="giant_tree_taiga">Giant Tree Taiga</option>
                                <option value="minecraft:giant_tree_taiga_hills" data-i18n="giant_tree_taiga_hills">Giant Tree Taiga Hills</option>
                                <option value="minecraft:gravelly_mountains" data-i18n="gravelly_mountains">Gravelly Mountains</option>
                                <option value="minecraft:ice_spikes" data-i18n="ice_spikes">Ice Spikes</option>
                                <option value="minecraft:jungle" data-i18n="jungle">Jungle</option>
                                <option value="minecraft:jungle_edge" data-i18n="jungle_edge">Jungle Edge</option>
                                <option value="minecraft:jungle_hills" data-i18n="jungle_hills">Jungle Hills</option>
                                <option value="minecraft:lukewarm_ocean" data-i18n="lukewarm_ocean">Lukewarm Ocean</option>
                                <option value="minecraft:modified_badlands_plateau" data-i18n="modified_badlands_plateau">Modified Badlands Plateau</option>
                                <option value="minecraft:modified_gravelly_mountains" data-i18n="modified_gravelly_mountains">Modified Gravelly Mountains</option>
                                <option value="minecraft:modified_jungle" data-i18n="modified_jungle">Modified Jungle</option>
                                <option value="minecraft:modified_jungle_edge" data-i18n="modified_jungle_edge">Modified Jungle Edge</option>
                                <option value="minecraft:modified_wooded_badlands_plateau" data-i18n="modified_wooded_badlands_plateau">Modified Wooded Badlands Plateau</option>
                                <option value="minecraft:mountain_edge" data-i18n="mountain_edge">Mountain Edge</option>
                                <option value="minecraft:mountains" data-i18n="mountains">Mountains</option>
                                <option value="minecraft:mushroom_field_shore" data-i18n="mushroom_field_shore">Mushroom Field Shore</option>
                                <option value="minecraft:mushroom_fields" data-i18n="mushroom_fields">Mushroom Fields</option>
                                <option value="minecraft:nether_wastes" data-i18n="nether_wastes">Nether Wastes</option>
                                <option value="minecraft:ocean" data-i18n="ocean">Ocean</option>
                                <option value="minecraft:plains" data-i18n="plains">Plains</option>
                                <option value="minecraft:river" data-i18n="river">River</option>
                                <option value="minecraft:savanna" data-i18n="savanna">Savanna</option>
                                <option value="minecraft:savanna_plateau" data-i18n="savanna_plateau">Savanna Plateau</option>
                                <option value="minecraft:shattered_savanna" data-i18n="shattered_savanna">Shattered Savanna</option>
                                <option value="minecraft:shattered_savanna_plateau" data-i18n="shattered_savanna_plateau">Shattered Savanna Plateau</option>
                                <option value="minecraft:small_end_islands" data-i18n="small_end_islands">Small End Islands</option>
                                <option value="minecraft:snowy_beach" data-i18n="snowy_beach">Snowy Beach</option>
                                <option value="minecraft:snowy_mountains" data-i18n="snowy_mountains">Snowy Mountains</option>
                                <option value="minecraft:snowy_taiga" data-i18n="snowy_taiga">Snowy Taiga</option>
                                <option value="minecraft:snowy_taiga_hills" data-i18n="snowy_taiga_hills">Snowy Taiga Hills</option>
                                <option value="minecraft:snowy_taiga_mountains" data-i18n="snowy_taiga_mountains">Snowy Taiga Mountains</option>
                                <option value="minecraft:snowy_tundra" data-i18n="snowy_tundra">Snowy Tundra</option>
                                <option value="minecraft:soul_sand_valley" data-i18n="soul_sand_valley">Soul Sand Valley</option>
                                <option value="minecraft:stone_shore" data-i18n="stone_shore">Stone Shore</option>
                                <option value="minecraft:sunflower_plains" data-i18n="sunflower_plains">Sunflower Plains</option>
                                <option value="minecraft:swamp" data-i18n="swamp">Swamp</option>
                                <option value="minecraft:swamp_hills" data-i18n="swamp_hills">Swamp Hills</option>
                                <option value="minecraft:taiga" data-i18n="taiga">Taiga</option>
                                <option value="minecraft:taiga_hills" data-i18n="taiga_hills">Taiga Hills</option>
                                <option value="minecraft:taiga_mountains" data-i18n="taiga_mountains">Taiga Mountains</option>
                                <option value="minecraft:tall_birch_forest" data-i18n="tall_birch_forest">Tall Birch Forest</option>
                                <option value="minecraft:tall_birch_hills" data-i18n="tall_birch_hills">Tall Birch Hills</option>
                                <option value="minecraft:the_end" data-i18n="the_end">The End</option>
                                <option value="minecraft:the_void" data-i18n="the_void">The Void</option>
                                <option value="minecraft:warm_ocean" data-i18n="warm_ocean">Warm Ocean</option>
                                <option value="minecraft:warped_forest" data-i18n="warped_forest">Warped Forest</option>
                                <option value="minecraft:wooded_badlands_plateau" data-i18n="wooded_badlands_plateau">Wooded Badlands Plateau</option>
                                <option value="minecraft:wooded_hills" data-i18n="wooded_hills">Wooded Hills</option>
                                <option value="minecraft:wooded_mountains" data-i18n="wooded_mountains">Wooded Mountains</option>
                            </select><br />
                            <div v-if="msgOptions[index].biome=='custom'">
                                Custom Biome:<br /><input type="text" v-model="msgOptions[index].biomeOther" placeholder="i.e. mypack:mybiome" />
                            </div>
                        </div>
                    </div>
                </div>
            </section>
            <!--NOT PART OF EACH FILE'S LOOP-->
            <section v-if="msgOptions.length > 0 && info==null">
                <br />
                <button class="btn" @click="saveMSG" :disabled="disableGenerateButton">Generate!</button>
            </section>
        </div>
        <br />
        <div v-if="info!==null">
            <!-- <button class="btn" @click="downloadPack()">Download Pack!</button> -->
            <h2>STRUCTURE GENERATION COMPLETE!</h2>
            <h3>How this works:</h3>
            <strong>1. First you'll need to have JDawgtor's Structure Generator data pack installed.</strong>
            <br /><a class="link" target="_blank" href="https://minecraft.gamepedia.com/Tutorials/Installing_a_data_pack">Learn how to install data packs. (Minecraft Wiki)</a><br />
            <br /><a class="btn" :href="'http://'+hostURL+'/sg/StructureGenerator-v'+msgOptions.sgver+'.zip'" download="JDawgtorStructureGenerator.zip">Download Structure Generator</a>
            &nbsp;<span class="notice">NOTE: We added the correct pack for your Minecraft version.</span>
            <br />
            <br /><strong>2. You'll need this custom structure data pack we put together special just for you:</strong>
            <br /><br /><a class="btn" :href="'http://'+hostURL+'/sg/JDawgtorsStructureGenDataPack-'+userGUID+'.zip'" download="JDawgtorCustomStructurePack.zip">Download Custom Structure Pack</a>
            &nbsp;<span class="notice">NOTE: Each pack is customized with your structures (for your Minecraft version).</span>
            <br /><br /><strong>Then you can reset or regenerate:</strong>
            <br /><br />
            <span v-if="msgOptions.length > 0 && info!==null">
                <button class="btn" @click="saveMSG" :disabled="disableGenerateButton">Regenerate!</button>
            </span>
            &nbsp;&nbsp;&nbsp;<a class="btn" onclick="location.href='/structure'">Reset</a>
            <br /><br />
            <h3>Retention:</h3>
            <p>
                We can only keep the generated files around for a short time. 
                <br />Please drag this link to your bookmarks so you can access it later. 
                <br /><br />
                &nbsp;&gt;&nbsp;<a class="link" :href="'http://'+hostURL+'/sg/JDawgtorsStructureGenDataPack-'+userGUID+'.zip'" title="JDawgtor SG Custom Data Pack">JDawgtor Minecraft SG DataPack (custom)</a>
                <br /><br />
                We will delete your pack file after a week to keep the server healthy. Feel free to come back and create a new pack anytime.
            </p>
        </div>
        <div v-if="info==null">
            <a class="btn" :href="'http://'+hostURL+'/sg/StructureGenerator-v1.3.1.zip'" download="JDawgtorStructureGenerator.zip">Download Latest Structure Generator</a>
            <div class="notice" style="margin-top: 5px;">Just in case you just need the Generator</div>
        </div>
        <br /><br />
        <!--
         {{info}}
        {{msgOptions}}
        -->
    </div>
</template>

<script>
import axios from 'axios';
import Swal from 'sweetalert2'
const STATUS_INITIAL = 0, STATUS_SAVING = 1, STATUS_SUCCESS = 2, STATUS_FAILED = 3;

export default {
  metaInfo: {
    title: 'Structure Gen'
  },
  data(){
      return{
        sgverEnabled:true,
        hostURL: 'api.jdawgtor.com',
        userGUID: null,
        info: null,
        sgver: "1.3.1",
        msgInitValues: {
            "placement": null, "yRangeMin": null,"yRangeMax": null,"posx": 0, "posy": 0, "posz": 0, "rotation": null, "weight": 50,"structName":"","dimension": null,"dimensionOther": "","biome":null,"biomeOther":""
        },
        msgOptions: [],
        msgDefaults: { 
            "placement": "surface", "posx": 0, "posy": 0, "posz": 0, "rotation": "yes", "weight": "50","dimension": null,"dimensionOther": "","biome":null,"biomeOther":"" 
        },
        uploadedFiles: [],
        uploadError: null,
        currentStatus: 0,
        uploadFieldName: 'files',
        disableGenerateButton: false
      }
  },
  computed:{
      isInitial() {return this.currentStatus === STATUS_INITIAL;},
      isSaving() {return this.currentStatus === STATUS_SAVING;},
      isSuccess() {return this.currentStatus === STATUS_SUCCESS;},
      isFailed() {return this.currentStatus === STATUS_FAILED;}
  },
  methods:{
      hostURLCalc() {
          var host = window.location.hostname;
          console.log(host);
          if(host.indexOf('localhost')!==-1){
              this.hostURL = 'localhost:3001';
          }
      },
      reset() {
        this.currentStatus = STATUS_INITIAL;
        this.uploadedFiles = [];
        this.uploadError = null;
      },
      saveFileUploads(formData) {
        this.currentStatus = STATUS_SAVING;
        axios.post('http://'+this.hostURL+'/api/v1/upload',formData,{headers: { "x-user-guid": this.userGUID }})
          .then(x => {
            this.uploadedFiles = [].concat(x.data);
            this.currentStatus = STATUS_SUCCESS;
          })
          .catch(err => {
            this.uploadError = err.response;
            this.currentStatus = STATUS_FAILED;
          });
      },
      genGUID(){
        return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
            var r = Math.random() * 16 | 0, v = c == 'x' ? r : (r & 0x3 | 0x8);
            return v.toString(16);
        });
      },
      filesChange(fieldName, fileList) {
        const formData = new FormData();
        this.sgverEnabled = false
        
        if (!fileList.length) return;
        Array
          .from(Array(fileList.length).keys())
          .map(x => {
            formData.append(fieldName, fileList[x], fileList[x].name);
            this.msgOptions.push({filename: fileList[x].name,userGUID:this.userGUID,sgver:this.sgver,...this.msgInitValues});
          });
        this.saveFileUploads(formData);
      },
      removeFile(filename){
          console.log('remove '+filename);
          var fileIndex = this.filesToUpload.indexOf(filename);
          //console.log(fileIndex);
          if (fileIndex > -1) {
            this.filesToUpload.splice(fileIndex,1)
          }
          this.msgOptions.splice(fileIndex,1);
          //console.log(this.filesToUpload);
          //this.filesToUpload = newFiles;
      },
      saveMSG(){
            this.info = null;
            var msgOpts = this.msgOptions;
            var msgDefaults = this.msgDefaults;
            var requiredValuesList = [];
            var formatValuesList = [];
            var requiredErrcnt = 0, formatErrcnt, filecnt = 0, fileErrCnt = 0, fileFormatErrCnt = 0;
            msgOpts.forEach(thisfile => {
                thisfile.requiredValuesList = [];
                thisfile.formatValuesList = [];
                if(thisfile){
                    if(thisfile.posx==null){
                        thisfile.requiredValuesList.push('PosX');
                        requiredErrcnt++;
                    }
                    if(thisfile.posy==null){
                        thisfile.requiredValuesList.push('PosY');
                        requiredErrcnt++;
                    }
                    if(thisfile.posz==null){
                        thisfile.requiredValuesList.push('PosZ');
                        requiredErrcnt++;
                    }
                    if(thisfile.placement=='y_range'){
                        console.log(thisfile.yRangeMin,thisfile.yRangeMax);
                        if(thisfile.yRangeMin==null){
                            thisfile.requiredValuesList.push('YRangeMin');
                            requiredErrcnt++;
                        }else{
                            // IF NOT NULL, CHECK FORMAT
                            if(thisfile.yRangeMin!==null){
                                if(thisfile.yRangeMin.match(/^[\d]{1,3}$/g)==null || thisfile.yRangeMin < 0){
                                    thisfile.formatValuesList.push('YRangeMin');
                                    formatErrcnt++;
                                    fileFormatErrCnt++;
                                }
                                // console.log(thisfile.yRangeMin,thisfile.yRangeMin.match(/^[\d]{1,3}$/g));
                            }
                        }
                        if(thisfile.yRangeMax==null){
                            thisfile.requiredValuesList.push('YRangeMax');
                            requiredErrcnt++;
                        }else{
                            // IF NOT NULL, CHECK FORMAT
                            if(thisfile.yRangeMax!==null){
                                if(thisfile.yRangeMax.match(/^[\d]{1,3}$/)==null || thisfile.yRangeMax > 255){
                                    thisfile.formatValuesList.push('YRangeMax');
                                    formatErrcnt++;
                                    fileFormatErrCnt++;
                                }
                                // console.log(thisfile.yRangeMax,thisfile.yRangeMax.match(/^[\d]{1,3}$/));
                            }
                        }

                        if(parseInt(thisfile.yRangeMin,10) > parseInt(thisfile.yRangeMax,10)){
                            thisfile.formatValuesList.push('YRangeMin to be less than YRangeMax to be ');
                            formatErrcnt++;
                            fileFormatErrCnt++;
                        }
                    }
                }
                if(requiredErrcnt == 0){
                    //console.log('set defaults');
                    // if required fields are good, we fill in optional values with defaults
                    if(!thisfile.sgver){thisfile.sgver = msgDefaults.sgver;}
                    if(!thisfile.placement){thisfile.placement = msgDefaults.placement;}
                    if(!thisfile.rotation){thisfile.rotation = msgDefaults.rotation;}
                    if(!thisfile.weight){thisfile.weight = msgDefaults.weight;}
                    // SEND IT!
                }else{
                    fileErrCnt++;
                }


                //console.log('thisfile: ',thisfile);
                filecnt++;
            });
            var alertText = "";
            if(fileErrCnt > 0){
                msgOpts.forEach(fileWithErrs => {
                    alertText += fileWithErrs.filename + ` needs ${fileWithErrs.requiredValuesList.join(',')} values.\r\n`;
                    fileWithErrs.requiredValuesList = null;
                });
            }
            if(fileFormatErrCnt > 0){
                msgOpts.forEach(fileWithErrs => {
                    alertText += fileWithErrs.filename + ` needs ${fileWithErrs.formatValuesList.join(',')} formatted correctly.\r\n`;
                    fileWithErrs.formatValuesList = null;
                });
            }
            if(fileErrCnt == 0 && fileFormatErrCnt == 0){
                console.log('Success Du Fromage!');
                axios.post('http://'+this.hostURL+'/api/v1/structure',msgOpts).then(response => (this.info = response.data))
            }else{
                Swal.fire({
                    icon: 'warning',
                    title: 'Do you even SHIFT bro? Something is off...',
                    text: alertText
                })
            }
            //this.disableGenerateButton = true;
      }
  },
  mounted(){
    this.reset();
    this.userGUID = this.genGUID();
    this.msgOptions.sgver = "1.3.1";
    this.hostURLCalc();
  }
}
</script>

<style>
    label{padding-right: 10px;}
    h1{margin-bottom: 10px;}
    section{margin: 10px 10px 0 10px;}
    input,input[type=text],select{font-size: 0.9rem; padding: 3px 5px; border-radius: 3px; border: 0; background: #154eb7; outline: none; color: #FFF;}
    .page-header{display: flex; flex-direction: row;  align-items: center; justify-content: center; padding-top: 10px;}
    .section-title{margin-bottom: 5px; margin-top: 0; font-size: 1.2rem; border-bottom: 1px solid #FFF;}
    .file-title{display: flex; flex-direction: row; border-radius: 5px;}
    .short{width: 50px;}
    .notice{font-size: 0.8rem; color: #000; padding: 0px; text-shadow: none;}
    .btn{cursor: pointer; color: #fff; text-shadow: 2px 2px 4px #000; background-image: linear-gradient(to right, #fcb9b9, #ffc0ac, #fecaa1, #f3d59b, #e2e29e, #d1eaab, #c0f0bb, #b2f5ce, #b1f6e0, #b8f5ee, #c4f3f6, #d4f1f8); font-size: 1.3rem; border: none; border-radius: 5px; padding: 5px 10px; text-decoration: none;}
    .btn:hover{color: #000; background-image: linear-gradient(to left, #fcb9b9, #ffc0ac, #fecaa1, #f3d59b, #e2e29e, #d1eaab, #c0f0bb, #b2f5ce, #b1f6e0, #b8f5ee, #c4f3f6, #d4f1f8);}
    .cols{display: flex; flex-direction: row; padding-left: 20px; margin-top: 10px;}
    .col{padding: 0 30px 0 0;}
    .title1{width: 60%; padding-left: 10px; border-radius: 5px 0 0 5px;}
    .title2{width: 20%;}
    .title3{width: 20%; padding-right: 10px; border-radius: 0 5px 5px 0;}
    .red-bg{background: #FCB9B9;}
    .green-bg{background: #C1F2B1;}
    .blue-bg{background: #D4F1F8;}
    .progress{width: 60px; text-align: center;}
    ::-webkit-input-placeholder{color: #000; font-style: italic}
    :-ms-input-placeholder {color: #000; font-style: italic}
    ::placeholder {color: #000; font-style: italic}
    .link, .link:hover,.link:visited{color: #FFF; text-decoration: underline;}
    .swal2-content,.swal2-title,.swal2-header, .swal2-icon{text-shadow: none;}
    .swal2-actions > button.swal2-styled{color: #000; background-image: linear-gradient(to right, #fcb9b9, #ffc0ac, #fecaa1, #f3d59b, #e2e29e, #d1eaab, #c0f0bb, #b2f5ce, #b1f6e0, #b8f5ee, #c4f3f6, #d4f1f8);}
    .swal2-actions > button:hover{
        background-image: linear-gradient(to left, #fcb9b9, #ffc0ac, #fecaa1, #f3d59b, #e2e29e, #d1eaab, #c0f0bb, #b2f5ce, #b1f6e0, #b8f5ee, #c4f3f6, #d4f1f8) !important;
    }
    .bitpack{width: 440px; margin-top: 5px; margin-left: 380px;}
/* FILE UPLOAD */
    .dropbox {
        outline: 2px dashed grey;
        outline-offset: -10px;
        background-image: linear-gradient(to right, #fcb9b9, #ffc0ac, #fecaa1, #f3d59b, #e2e29e, #d1eaab, #c0f0bb, #b2f5ce, #b1f6e0, #b8f5ee, #c4f3f6, #d4f1f8);
        color: gray;
        padding: 10px 10px;
        min-height: 100px;
        position: relative;
        cursor: pointer;
        text-shadow: none;
    }
    .input-file {opacity: 0; width: 100%; height: 100px; position: absolute; cursor: pointer;}
    .dropbox:hover {
        background-image: linear-gradient(to left, #fcb9b9, #ffc0ac, #fecaa1, #f3d59b, #e2e29e, #d1eaab, #c0f0bb, #b2f5ce, #b1f6e0, #b8f5ee, #c4f3f6, #d4f1f8);
    }
    .dropbox p {font-size: 1.2em; text-align: center; padding: 10px 0;}

/* Slider */
.slidecontainer {
  width: 100%;
  display: flex;
  flex-direction: row;
}

.slider {
  -webkit-appearance: none;
  appearance: none;
  width: 100%;
  height: 20px;
  background: #154eb7;
  outline: none;
  opacity: 0.7;
  -webkit-transition: .2s;
  transition: opacity .2s;
}

.slider:hover {
  opacity: 1;
}

.slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 20px;
  height: 20px;
  background: #FCB9B9;
  cursor: pointer;
}

.slider::-moz-range-thumb {
  width: 25px;
  height: 25px;
  background: #FCB9B9;
  cursor: pointer;
} 
// Animated fadein
.animated {
    background-repeat: no-repeat;
    background-position: left top;
    margin-bottom:0px;
    -webkit-animation-duration: 3s;
    -moz-animation-duration: 3s;
    animation-duration: 3s;
    -webkit-animation-fill-mode: both;
    -moz-animation-fill-mode: both;
    animation-fill-mode: both;
}

@-webkit-keyframes fadeIn {
    0% {opacity: 0;}
    100% {opacity: 1;}
}

@keyframes fadeIn {
    0% {opacity: 0;}
    100% {opacity: 1;}
}

.fadeIn {
    -webkit-animation-name: fadeIn;
    animation-name: fadeIn;
}
.flex{display: flex; flex-direction: row;}
</style>