<template>
  <div class="index-page">
    <div class="stats-container">
      <div
        class="stat-wrapper"
        v-for="(stat, statIdx) in playerData.stats"
        :key="stat.name"
      >
        <div class="stat-item data-item">
          <span class="stat-name">{{ stat.name }}:</span>
          <span class="stat-value">{{ stat.value }}</span>
          <div class="act-box">
            <span class="act-btn" @click="changeStat(1, statIdx)">加</span>
            <span class="act-btn" @click="changeStat(-1, statIdx)">减</span>
          </div>
        </div>
        <div class="ablt-wrapper">
          <div
            class="ablt-item data-item"
            v-for="(ability, abltIdx) in stat.abilities"
            :key="ability.name"
          >
            <span class="ablt-name">{{ ability.name }}:</span>
            <span class="ablt-value">{{ ability.value }}</span>
            <div class="act-box">
              <span class="act-btn" @click="changeAbility(1, statIdx, abltIdx)"
                >加</span
              >
              <span class="act-btn" @click="changeAbility(-1, statIdx, abltIdx)"
                >减</span
              >
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="overview-box">
      <span class="label">属性总和：</span>
      <span class="value">{{ totalStats }}</span>
    </div>
    <div class="overview-box">
      <span class="label">能力值合计：</span>
      <span class="value">{{ totalAbilityValue }}</span>
    </div>
    <div class="action-box">
      <span class="btn" @click="previewExport">导出</span>
      <span class="btn" @click="openExportModal">导入</span>
    </div>
    <div class="modal-container" v-if="exportModalVisible">
      <textarea class="input" name="" cols="30" rows="10" :value="inputValue">
      </textarea>

      <div
        class="action-box"
        style="display: flex; align-item: center; justify-content: center"
      >
        <span class="btn" v-if="!isExport" @click="excuteImport">导入</span>
        <span class="btn" v-if="!isExport" @click="useLocalData">加载缓存</span>
        <span class="btn" v-if="isExport" @click="saveToLocal">存入缓存</span>
        <span class="btn" @click="closeExportModal">关闭</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      exportModalVisible: false,
      // changeDataVisibel: false,
      isExport: false,
      inputValue: "111",
      // totalStats: 0
      // totalAbilityValue: 0,
      playerData: {
        stats: [
          {
            name: "stat1",
            value: 0,
            abilities: [
              {
                name: "ab1",
                value: 0,
              },
              {
                name: "ab2",
                value: 0,
              },
              {
                name: "ab3",
                value: 0,
              },
              {
                name: "ab4",
                value: 0,
              },
              {
                name: "ab5",
                value: 0,
              },
              {
                name: "ab6",
                value: 0,
              },
            ],
          },
          {
            name: "stat2",
            value: 2,
            abilities: [
              {
                name: "ab1",
                value: 2,
              },
              {
                name: "ab2",
                value: 2,
              },
              {
                name: "ab3",
                value: 0,
              },
            ],
          },
          {
            name: "stat3",
            value: 2,
            abilities: [
              {
                name: "ab1",
                value: 6,
              },
            ],
          },
          {
            name: "stat4",
            value: 2,
            abilities: [
              {
                name: "ab1",
                value: 0,
              },
            ],
          },
        ],
      },
    };
  },
  methods: {
    saveToLocal() {
      localStorage.setItem("playerData", JSON.stringify(this.playerData));
    },
    useLocalData() {
      // try {
      const str = localStorage.getItem("playerData");
      console.log("str", str);
      if (!str) {
        alert("没有缓存");
        return;
      }
      this.inputValue = str;
      // } catch (error) {
      // alert(error.message);
      // }
    },
    openExportModal() {
      this.exportModalVisible = true;
    },

    closeExportModal() {
      this.exportModalVisible = false;
      this.isExport = false;
      this.inputValue = "";
    },

    excuteImport() {
      this.playerData = JSON.parse(this.inputValue);
      this.closeExportModal();
      // this.inputValue = ""
    },

    previewExport() {
      this.openExportModal();
      this.isExport = true;
      this.inputValue = JSON.stringify(this.playerData);
    },
    changeStat(diff, statIndex) {
      // if(this.playerData.stats[statIndex].value <= 0){
      //   return
      // }
      this.playerData.stats[statIndex].value += diff;
    },
    changeAbility(diff, statIdx, abltIdx) {
      // if(this.playerData.stats[statIdx].abilities[abltIdx].value <= 0){
      //   return
      // }
      this.playerData.stats[statIdx].abilities[abltIdx].value += diff
    },
  },

  computed: {
    totalStats() {
      return this.playerData.stats.reduce((ttl, crrt) => {
        return ttl + crrt.value;
      }, 0);
    },
    totalAbilityValue() {
      return this.playerData.stats.reduce((total, current) => {
        return (
          total +
          current.abilities.reduce((ttl, crrt) => {
            return ttl + crrt.value;
          }, 0)
        );
      }, 0);
    },
  },
  name: "index",
  components: {},
};
</script>

<style lang="scss">
.index-page {
  .stats-container {
    border: 1px solid #666;

    padding: 10px;
    .stat-wrapper {
      padding: 5px;
      display: flex;

      .stat-item {
        border: 1px solid #666;
        // padding: 5px;
        display: flex;
        flex-wrap: nowrap;
        justify-content: center;
        align-items: center;
        width: 20%;
        .stat-name {
        }
        .stat-value {
          padding: 3px;
        }
      }
      .ablt-wrapper {
        width: 80%;
        // padding: 5px;
        display: flex;
        flex-wrap: wrap;
        border: 1px solid #666;
        .ablt-item {
          // padding: 5px;
          border: 1px solid #666;
          display: flex;
          flex-wrap: nowrap;
          .ablt-name {
          }
          .ablt-value {
            padding: 3px;
          }
        }
      }
    }
  }
  .action-box {
    .btn {
      padding: 5px;
      border: 1px solid;
      margin-right: 10px;
    }
  }

  .action-box,
  .overview-box {
    padding: 5px;
  }
  .modal-container {
    .btn {
      background-color: #ccc;
      margin-right: 30px;
    }
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: 2;
    background-color: rgba(0, 0, 0, 0.7);
    .input {
      width: 80%;
      display: block;
      margin: 20px auto;
      margin-top: 50px;
    }
  }
}
.act-box {
  // padding: 0 3px;
  display: flex;
  flex-direction: column;
  height: 100%;
  justify-content: space-around;
  .act-btn {
    background-color: #ccc;
    border: 1px solid #999;
    padding: 5px;

    font-size: 12px;
  }
}
.data-item {
  align-items: center;
  justify-content: center;
}
</style>
