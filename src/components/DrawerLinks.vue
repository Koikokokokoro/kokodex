<template>
  <div class="drawer-links">
    <div v-for="(link, index) in links" :key="index" class="link-item">
      <div class="link-wrapper">
        <a :href="link.url" class="link" target="_blank">
          <img :src="link.icon || 'https://via.placeholder.com/30'" alt="Icon" class="icon" />
          <span class="name">{{ link.name }}</span>
        </a>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, watch } from 'vue';

export default defineComponent({
  name: 'DrawerLinks',
  setup() {
    const links = ref<{ name: string; url: string; icon?: string }[]>([]);
    const showModal = ref(false);
    const modalType = ref<'add' | 'edit'>('add');
    const currentLink = ref({ name: '', url: '', icon: '' });
    const editingIndex = ref<number | null>(null);

    // 加载本地存储中的链接
    const loadLinksFromStorage = () => {
      const savedLinks = localStorage.getItem('drawerLinks');
      links.value = savedLinks ? JSON.parse(savedLinks) : getDefaultLinks();
    };

    // 保存链接到本地存储
    const saveLinksToStorage = () => {
      localStorage.setItem('drawerLinks', JSON.stringify(links.value));
    };

    // 默认链接
    const getDefaultLinks = () => [
      { name: 'GitHub', url: 'https://github.com/Koikokokokoro', icon: '/icons/github.svg' },
      { name: 'Alist', url: 'https://pan.koikokokokoro.icu', icon: '/icons/alist.svg' },
    ];

    // 在组件挂载时加载链接数据
    onMounted(() => {
      loadLinksFromStorage();
    });

    // 监听 links 的变化并自动保存到本地存储
    watch(links, saveLinksToStorage, { deep: true });

    return {
      links,
      showModal,
      modalType,
      currentLink,
    };
  },
});
</script>

<style scoped>
.drawer-links {
  display: flex;
  gap: 20px;
  align-items: center;
}

.link-item {
  position: relative;
}

.link-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.link {
  text-decoration: none;
  color: white;
  text-align: center;
}

.icon {
  width: 30px;
  height: 30px;
  margin-bottom: 5px;
}

.name {
  font-size: 0.9em;
}
</style>


<style scoped>
.drawer-links {
  display: flex;
  gap: 50px;
  /* border-radius: 25px; */
}

.link {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-decoration: none;
  color: white;
}

.icon {
  width: 50px;
  height: 50px;
}

.name {
  margin-top: 10px;
  font-size: 0.9em;
}
</style>
