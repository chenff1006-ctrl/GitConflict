<template>
  <div class="video-player-container" @click="handleContainerClick">
    
    <!-- 顶部控制栏 新编辑1123-->
    <div class="top-control-bar">
      <div class="control-buttons">
        <button class="control-btn play-btn" @click="playVideo">
          <i class="icon-play">
            <svg t="1757491075037" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="7180" width="14" height="14"><path d="M780.8 475.733333L285.866667 168.533333c-27.733333-17.066667-64 4.266667-64 36.266667v614.4c0 32 36.266667 53.333333 64 36.266667l492.8-307.2c29.866667-14.933333 29.866667-57.6 2.133333-72.533334z" fill="#ffffff" p-id="7181"></path></svg>
          </i> 播放 按钮A
        </button>
        <button class="control-btn pause-btn" @click="pauseVideo">
          <i class="icon-pause">
            <svg t="1757491109090" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="8235" width="14" height="14"><path d="M116.053333 146.773333h184.746667v730.026667H116.053333zM723.2 146.773333h184.746667v730.026667h-184.746667z" fill="#ffffff" p-id="8236"></path></svg>
          </i> 暂停 按钮A
        </button>
        <button class="control-btn resume-btn" @click="resumeVideo">
          <i class="icon-resume">
            <svg t="1757491154207" class="icon" viewBox="0 0 1036 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="9366" width="14" height="14"><path d="M159.5472 86.45759999c-47.2416 0-88.5792 42.4464-88.5792 90.9648v667.1424c0 48.5184 41.3376 90.9648 88.5792 90.9648s88.5792-42.4464 88.5792-90.9648V177.42239999c-0.0912-48.5232-41.424-90.9648-88.5792-90.9648z m773.3952 497.7936l-407.1696 332.4816c-35.4 28.9008-83.8224 18.264-108.1536-23.784-8.9136-15.4032-13.6848-33.6432-13.6848-52.3296V175.64639999c0-51.0192 34.8288-92.3808 77.784-92.376 15.7344 0 31.0896 5.6688 44.0544 16.2528l407.1696 332.4816c35.4 28.9056 44.3616 86.424 20.0208 128.4672-5.3904 9.3168-12.1728 17.3808-20.0208 23.7792z" p-id="9367" fill="#ffffff"></path></svg>
          </i> 继续
        </button>
        <button class="control-btn stop-btn" @click="stopVideo">
          <i class="icon-stop">
            <svg t="1757491184787" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="10465" width="14" height="14"><path d="M835.5 815.5v-607c0-11-9-20-20-20h-607c-11 0-20 9-20 20v607c0 11 9 20 20 20h607c11 0 20-9 20-20z" fill="#ffffff" p-id="10466"></path></svg>
          </i> 结束 按钮A
        </button>
        <div class="volume-control-wrapper" @click.stop>
          <button class="control-btn volume-btn" @click="toggleVolumeControl">
            <i class="icon-volume">
              <svg t="1757491227555" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="12547" width="18" height="18"><path d="M710.4 892.8V134.4l-246.4 246.4H249.6v265.6h214.4z" fill="#ffffff" p-id="12548"></path></svg>
            </i> 音量
          </button>
          <div v-if="showVolumeControl" class="volume-slider-popup">
            <input 
              type="range" 
              min="0" 
              max="100" 
              v-model="volumeLevel" 
              @input="onVolumeChange"
              class="volume-slider"
            >
            <span class="volume-value">{{ volumeLevel }}%</span>
          </div>
        </div>
        <button class="control-btn power-on-btn" @click="powerOn">
          <i class="icon-power">
         <svg t="1757491580895" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="35313" width="16" height="16"><path d="M512 64a448 448 0 1 0 0 896A448 448 0 0 0 512 64z m0 820.004571A372.004571 372.004571 0 1 1 512 139.995429a372.004571 372.004571 0 0 1 0 744.009142z m-45.714286-197.778285l219.428572-126.683429a54.857143 54.857143 0 0 0 0-95.085714l-219.428572-126.683429a54.857143 54.857143 0 0 0-82.285714 47.542857v253.366858a54.857143 54.857143 0 0 0 82.285714 47.542857z" fill="#ffffff" p-id="35314"></path></svg>
          </i> 开机 按钮A
        </button>
        <button class="control-btn power-off-btn" @click="powerOff">
          <i class="icon-power-off">
            <svg t="1757491316332" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="19099" width="14" height="14"><path d="M764 201q45 32 80 72.5t60 87.5 38 99.5 13 106.5q0 93-35.5 174.5t-96 142-142 96-174.5 35.5q-92 0-173.5-35.5t-142.5-96-96-142-35-174.5q0-53 12.5-104t35.5-97 57-86 76-72q22-16 47.5-12t41.5 25 12 47-25 42q-63 46-96.5 113t-33.5 144q0 66 25 124.5t68.5 102 102 69 124.5 25.5 124.5-25.5 102-69 69-102 25.5-124.5q0-78-36-147.5t-101-114.5q-22-15-26.5-41t10.5-48q15-21 41-25.5t48 10.5zM507 570q-26 0-44.5-18.5t-18.5-44.5v-380q0-26 18.5-45t44.5-19q27 0 45.5 19t18.5 45v380q0 26-18.5 44.5t-45.5 18.5z" p-id="19100" fill="#ffffff"></path></svg>
          </i> 关机
        </button>
        <button class="control-btn restart-btn" @click="restart">
          <i class="icon-restart">
            <svg t="1757491411854" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="28741" width="18" height="18"><path d="M139.704889 627.342222l-10.709333 52.152889a37.048889 37.048889 0 0 1-45.027556 24.177778 32.170667 32.170667 0 0 1-26.353778-38.570667l38.257778-115.740444a36.636444 36.636444 0 0 1 17.251556-22.385778 37.148444 37.148444 0 0 1 27.875555-3.712l124.444445 36.551111a36.778667 36.778667 0 0 1 22.357333 17.408c4.878222 8.561778 6.158222 18.716444 3.555555 28.231111a30.506667 30.506667 0 0 1-39.182222 24.945778l-37.674666-10.780444a319.317333 319.317333 0 0 0 614.172444-53.447112c-0.526222 10.723556 13.084444 0.455111 29.198222 0.455112 11.079111 0 21.560889 4.949333 28.586667 13.511111 7.04 8.590222 9.898667 19.882667 7.793778 30.791111A392.647111 392.647111 0 0 1 139.704889 627.342222z m282.922667 68.124445a32.711111 32.711111 0 0 1-13.724445-27.178667V357.603556a32.440889 32.440889 0 0 1 13.681778-27.192889 22.883556 22.883556 0 0 1 26.851555 0.853333l221.112889 155.264a34.289778 34.289778 0 0 1 0 52.764444L449.450667 694.599111a22.869333 22.869333 0 0 1-26.88 0.896h0.056889zM887.04 493.795556h0.142222l-55.125333-14.848-76.046222-25.685334a37.048889 37.048889 0 1 1 19.342222-71.452444l37.916444 15.416889a319.459556 319.459556 0 0 0-232.32-202.524445 280.078222 280.078222 0 0 0-66.730666-6.983111A319.872 319.872 0 0 0 205.937778 423.082667a38.001778 38.001778 0 0 1-36.792889 31.786666 37.048889 37.048889 0 0 1-34.986667-48.924444 392.860444 392.860444 0 0 1 754.645334-16.654222l4.792888-53.688889a35.2 35.2 0 0 1 46.179556-26.112c9.486222 2.545778 17.564444 8.775111 22.414222 17.322666 4.892444 8.533333 6.200889 18.688 3.612445 28.202667l-33.365334 112.597333a36.664889 36.664889 0 0 1-17.208889 22.428445 37.262222 37.262222 0 0 1-28.16 3.697778v0.071111z" fill="#ffffff" p-id="28742"></path></svg>
          </i> 重启
        </button>
        <button class="control-btn record-btn" @click="showRecord">
          <i class="icon-record">
            <svg t="1757491454122" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="29894" width="16" height="16"><path d="M512 73.142857a438.857143 438.857143 0 1 0 438.857143 438.857143 438.857143 438.857143 0 0 0-438.857143-438.857143z m190.171429 504.685714H446.171429V263.314286H512V512h190.171429z" fill="#ffffff" p-id="29895"></path></svg>
          </i> 播放记录 按钮A
        </button>
      </div>
    </div>
    
    <!-- 播放信息 -->
    <div class="playback-info">
      <div class="time-highlight">
        <span class="media-title">{{ mediaTitle }}</span>
        <span class="time-separator">|</span>
        <span class="current-time">当前播放时长：{{ currentTime }}</span>
        <span class="time-separator">|</span>
        <span class="estimated-time">预计播放时长：{{ estimatedTime }}</span>
      </div>
      
    </div>

    <!-- 中间视频播放区域 -->
    <div class="video-display-area">
      <div class="video-placeholder">
        <p class="video-text">文字</p>
      </div>
    </div>

    <!-- 底部播放设置区域 -->
    <div class="bottom-settings-area">
      <h3>切换按钮A</h3>
      <div class="room-selection">
        <span 
          v-for="(room, index) in rooms" 
          :key="index" 
          :class="{ active: selectedRoom === room }"
          @click="selectRoom(room)"
          class="room-option"
        >
          {{ room }}
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VideoPlayer',
  data() {
    return {
      currentTime: '00:00:00',
      estimatedTime: '02:30:00',
      estimatedFinish: '18:30',
      selectedRoom: '房间室A',
      rooms: ['房间室A', '房间室B按钮A', '房间室C', '房间室D', '房间室E按钮A'],
      showVolumeControl: false,
      volumeLevel: 80,
      mediaTitle: '播放素材.mp4'
    }
  },
methods: {
    playVideo() {
      console.log('播放素材')
    },
    pauseVideo() {
      console.log('暂停播放')
    },
    resumeVideo() {
      console.log('继续播放')
    },
    stopVideo() {
      console.log('结束播放')
    },
    toggleVolumeControl() {
      this.showVolumeControl = !this.showVolumeControl;
      console.log('切换音量控制面板:', this.showVolumeControl);
    },
    handleContainerClick() {
      // 点击容器外部关闭音量控制面板
      if (this.showVolumeControl) {
        this.showVolumeControl = false;
        console.log('点击外部区域，关闭音量控制面板');
      }
    },
    onVolumeChange() {
      console.log('音量调整为:', this.volumeLevel + '%');
    },
    powerOn() {
      console.log('设备开机')
    },
    powerOff() {
      console.log('设备关机')
    },
    restart() {
      console.log('设备重启')
    },
    showRecord() {
      console.log('查看播放记录')
    },
    selectRoom(room) {
      this.selectedRoom = room
      console.log('选择房间:', room)
    }
  }
}
</script>

<style scoped>
.video-player-container {
  width: 100%;
  height: 800px;
  display: flex;
  flex-direction: column;
  font-family: Arial, sans-serif;
}

.top-control-bar,
.playback-info {
  flex-shrink: 0;
}

/* 顶部控制栏 */
.top-control-bar {
  width: 100%;
  height: 60px;
  background-color: #f8f9fa;
  border-bottom: 1px solid #dee2e6;
  display: flex;
  align-items: center;
  padding: 0 20px;
  box-sizing: border-box;
}

.control-buttons {
  display: flex;
  gap: 10px;
}

.control-btn {
  width: 80px;
  height: 30px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 5px;
  font-size: 14px;
  transition: all 0.3s ease;
  background-color: transparent;
  color: black;
  white-space: nowrap;
  position: relative;
  overflow: hidden;
}

.control-btn:hover {
  opacity: 0.9;
  transform: translateY(-3px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
}

.control-btn:active {
  transform: translateY(-1px);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
}



.control-btn i {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 22px;
  height: 22px;
  border-radius: 50%;
  flex-shrink: 0;
}

.play-btn i {
  background-color: #007bff;
  color: white;
}

.pause-btn i {
  background-color: #6c757d;
  color: white;
}

.resume-btn i {
  background-color: #28a745;
  color: white;
}

.stop-btn i {
  background-color: #dc3545;
  color: white;
}

.volume-btn i {
  background-color: #17a2b8;
  color: white;
}

.power-on-btn i {
  background-color: #20c997;
  color: white;
}

.power-off-btn i {
  background-color: #fd7e14;
  color: white;
}

.restart-btn i {
  background-color: #6f42c1;
  color: white;
}

.record-btn i {
  background-color: #ffc107;
  color: #212529;
}

/* 音量控制 */
.volume-control-wrapper {
  position: relative;
  display: inline-block;
}

.volume-slider-popup {
  position: absolute;
  top: 35px;
  left: 0;
  background: white;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 10px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  z-index: 100;
  display: flex;
  align-items: center;
  gap: 10px;
}

.volume-slider {
  width: 100px;
  height: 5px;
  background: #ddd;
  border-radius: 5px;
  outline: none;
  -webkit-appearance: none;
}

.volume-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 15px;
  height: 15px;
  border-radius: 50%;
  background: #007bff;
  cursor: pointer;
}

.volume-value {
  font-size: 12px;
  color: #333;
  min-width: 30px;
}

.playback-info {
  padding: 10px 20px;
  color: black;
  background-color: #f8f9fa;
  border-bottom: 1px solid #dee2e6;
}

.time-highlight {
  display: flex;
  align-items: center;
  font-size: 16px;
  font-weight: bold;
  height: 22px;
  line-height: 1;
}

.media-title {
  color: #333;
  font-weight: bold;
  background-color: #e9ecef;
  padding: 2px 8px;
  border-radius: 4px;
  margin-right: 10px;
}

.current-time {
  color: #007bff;
}

.time-separator {
  color: #ccc;
  margin: 0 15px;
}

.estimated-time {
  color: #28a745;
  margin-right: 10px;
}

.completion-info {
  font-size: 14px;
  color: #6c757d;
}

/* 中间视频播放区域 */
.video-display-area {
  width: 100%;
  height: 600px;
  background-color: #343a40;
  display: flex;
  align-items: center;
  justify-content: center;
}

.video-placeholder {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  background-image: linear-gradient(45deg, #2c2c2c 25%, transparent 25%), 
                    linear-gradient(-45deg, #2c2c2c 25%, transparent 25%), 
                    linear-gradient(45deg, transparent 75%, #2c2c2c 75%), 
                    linear-gradient(-45deg, transparent 75%, #2c2c2c 75%);
  background-size: 20px 20px;
  background-position: 0 0, 0 10px, 10px -10px, -10px 0px;
}

.video-text {
  color: white;
  font-size: 48px;
  font-weight: bold;
  text-align: center;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

/* 底部播放设置区域 */
.bottom-settings-area {
  width: 100%;
  height: 140px;
  background-color: #f8f9fa;
  padding: 20px;
  box-sizing: border-box;
  border-top: 1px solid #dee2e6;
}

.bottom-settings-area h3 {
  margin: 0 0 15px 0;
  color: #333;
  font-size: 16px;
}

.room-selection {
  display: flex;
  gap: 15px;
  flex-wrap: wrap;
}

.room-option {
  color: #007bff;
  cursor: pointer;
  padding: 8px 15px;
  border: 1px solid #007bff;
  border-radius: 4px;
  font-size: 14px;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.room-option:hover {
  background-color: #007bff;
  color: white;
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 123, 255, 0.3);
}

.room-option.active {
  background-color: #007bff;
  color: white;
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 123, 255, 0.3);
}



</style>
