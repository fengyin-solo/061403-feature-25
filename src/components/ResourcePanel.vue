<template>
  <div class="resource-panel">
    <h3 class="panel-title">资源</h3>
    <div class="resources-grid">
      <div class="resource-item">
        <span class="resource-icon">🔥</span>
        <div class="resource-info">
          <span class="resource-name">热量</span>
          <div class="resource-bar-container">
            <div class="resource-bar" :style="{ width: heat + '%', background: getHeatColor() }"></div>
          </div>
          <span class="resource-value">{{ Math.round(heat) }}/100</span>
        </div>
      </div>
      <div class="resource-item">
        <span class="resource-icon">🪵</span>
        <div class="resource-info">
          <span class="resource-name">木头</span>
          <span class="resource-value-large">{{ wood }}</span>
        </div>
      </div>
      <div class="resource-item">
        <span class="resource-icon">🍖</span>
        <div class="resource-info">
          <span class="resource-name">食物</span>
          <span class="resource-value-large">{{ food }}</span>
        </div>
      </div>
      <div class="resource-item">
        <span class="resource-icon">🦊</span>
        <div class="resource-info">
          <span class="resource-name">兽皮</span>
          <span class="resource-value-large">{{ hide }}</span>
        </div>
      </div>
      <div class="resource-item tools-item">
        <span class="resource-icon">🔪</span>
        <div class="resource-info">
          <div class="tools-header">
            <span class="resource-name">工具</span>
            <span class="resource-value-large">{{ tools.length }}</span>
          </div>
          <div v-if="tools.length > 0" class="tools-durability-list">
            <div 
              v-for="(tool, index) in tools" 
              :key="tool.id || index" 
              class="tool-durability-item"
            >
              <span class="tool-index">#{{ index + 1 }}</span>
              <div class="tool-durability-bar-container">
                <div 
                  class="tool-durability-bar" 
                  :style="{ 
                    width: (tool.durability / tool.maxDurability * 100) + '%', 
                    background: getToolDurabilityColor(tool.durability, tool.maxDurability) 
                  }"
                ></div>
              </div>
              <span class="tool-durability-text">{{ tool.durability }}/{{ tool.maxDurability }}</span>
            </div>
          </div>
          <div v-else class="no-tools">
            <span class="no-tools-text">暂无工具，快去制作吧！</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  heat: { type: Number, default: 0 },
  wood: { type: Number, default: 0 },
  food: { type: Number, default: 0 },
  hide: { type: Number, default: 0 },
  tools: { type: Array, default: () => [] }
})

function getHeatColor() {
  if (props.heat > 60) return 'linear-gradient(to right, #ff6600, #ffcc00)'
  if (props.heat > 30) return 'linear-gradient(to right, #ff9933, #ffcc00)'
  return 'linear-gradient(to right, #cc3300, #ff6600)'
}

function getToolDurabilityColor(durability, maxDurability) {
  const ratio = durability / maxDurability
  if (ratio > 0.6) return 'linear-gradient(to right, #2ecc71, #27ae60)'
  if (ratio > 0.3) return 'linear-gradient(to right, #f39c12, #e67e22)'
  return 'linear-gradient(to right, #e74c3c, #c0392b)'
}
</script>

<style scoped>
.resource-panel {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 15px;
  padding: 20px;
  backdrop-filter: blur(10px);
  border: 2px solid rgba(255, 255, 255, 0.2);
}

.panel-title {
  color: white;
  font-size: 18px;
  margin-bottom: 15px;
  text-align: center;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

.resources-grid {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.resource-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px;
  background: rgba(0, 0, 0, 0.2);
  border-radius: 10px;
  transition: transform 0.2s;
}

.resource-item:hover {
  transform: translateX(5px);
  background: rgba(0, 0, 0, 0.3);
}

.tools-item {
  align-items: flex-start;
}

.resource-icon {
  font-size: 28px;
  width: 40px;
  text-align: center;
  flex-shrink: 0;
}

.resource-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 4px;
  min-width: 0;
}

.tools-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.resource-name {
  color: rgba(255, 255, 255, 0.8);
  font-size: 12px;
}

.resource-bar-container {
  height: 8px;
  background: rgba(0, 0, 0, 0.3);
  border-radius: 4px;
  overflow: hidden;
}

.resource-bar {
  height: 100%;
  border-radius: 4px;
  transition: width 0.3s ease;
}

.resource-value {
  color: white;
  font-size: 12px;
  font-weight: bold;
}

.resource-value-large {
  color: white;
  font-size: 24px;
  font-weight: bold;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

.tools-durability-list {
  display: flex;
  flex-direction: column;
  gap: 4px;
  margin-top: 4px;
}

.tool-durability-item {
  display: flex;
  align-items: center;
  gap: 6px;
}

.tool-index {
  font-size: 10px;
  color: rgba(255, 255, 255, 0.5);
  width: 20px;
  flex-shrink: 0;
}

.tool-durability-bar-container {
  flex: 1;
  height: 6px;
  background: rgba(0, 0, 0, 0.3);
  border-radius: 3px;
  overflow: hidden;
}

.tool-durability-bar {
  height: 100%;
  border-radius: 3px;
  transition: width 0.3s ease;
}

.tool-durability-text {
  font-size: 10px;
  color: rgba(255, 255, 255, 0.7);
  width: 36px;
  text-align: right;
  flex-shrink: 0;
}

.no-tools {
  padding: 4px 0;
}

.no-tools-text {
  font-size: 11px;
  color: rgba(255, 255, 255, 0.4);
  font-style: italic;
}
</style>
