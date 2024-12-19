<template>
  <div class="drawer-links">
    <div v-for="(link, index) in links" :key="index" class="link-item">
      <div class="link-wrapper">
        <a :href="link.url" class="link" target="_blank">
          <img :src="link.icon || 'https://via.placeholder.com/30'" alt="Icon" class="icon" />
          <span class="name">{{ link.name }}</span>
        </a>
        <button class="edit-btn" @click="openEditLinkModal(index)">✏️</button>
      </div>
    </div>
    <div class="add-link">
      <button @click="openAddLinkModal" class="add-btn">➕</button>
    </div>
    <div v-if="showModal" class="modal">
      <div class="modal-content">
        <h3>{{ modalType === 'add' ? '添加快捷链接' : '编辑快捷链接' }}</h3>
        <label>
          名称:
          <input type="text" v-model="currentLink.name" placeholder="请输入链接名称" />
        </label>
        <label>
          URL:
          <input type="text" v-model="currentLink.url" placeholder="请输入链接地址" />
        </label>
        <label>
          图标 URL (可选):
          <input type="text" v-model="currentLink.icon" placeholder="请输入图标地址" />
        </label>
        <div class="modal-actions">
          <button v-if="modalType === 'edit'" @click="deleteLink" class="delete-btn">删除</button>
          <button @click="saveLink" class="confirm-btn">确认</button>
          <button @click="closeModal" class="cancel-btn">取消</button>
        </div>
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
      { name: 'GitHub', url: 'https://github.com', icon: 'https://via.placeholder.com/30' },
      { name: 'Twitter', url: 'https://twitter.com', icon: 'https://via.placeholder.com/30' },
      { name: 'LinkedIn', url: 'https://linkedin.com', icon: 'https://via.placeholder.com/30' },
      { name: 'Blog', url: 'https://example.com', icon: 'https://via.placeholder.com/30' },
    ];

    const openAddLinkModal = () => {
      modalType.value = 'add';
      currentLink.value = { name: '', url: '', icon: '' };
      showModal.value = true;
    };

    const openEditLinkModal = (index: number) => {
      modalType.value = 'edit';
      editingIndex.value = index;
      currentLink.value = { ...links.value[index] };
      showModal.value = true;
    };

    const closeModal = () => {
      showModal.value = false;
      currentLink.value = { name: '', url: '', icon: '' };
      editingIndex.value = null;
    };

    const saveLink = () => {
      if (!currentLink.value.name || !currentLink.value.url) {
        alert('名称和 URL 为必填项！');
        return;
      }
      if (modalType.value === 'add') {
        links.value.push({ ...currentLink.value });
      } else if (modalType.value === 'edit' && editingIndex.value !== null) {
        links.value[editingIndex.value] = { ...currentLink.value };
      }
      saveLinksToStorage(); // 保存到本地存储
      closeModal();
    };

    const deleteLink = () => {
      if (editingIndex.value !== null) {
        links.value.splice(editingIndex.value, 1);
        saveLinksToStorage(); // 保存到本地存储
        closeModal();
      }
    };

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
      openAddLinkModal,
      openEditLinkModal,
      closeModal,
      saveLink,
      deleteLink,
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

/* 编辑按钮 */
.edit-btn {
  position: absolute;
  top: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.7);
  color: white;
  border: none;
  border-radius: 50%;
  padding: 5px;
  font-size: 0.8em;
  cursor: pointer;
  transition: background 0.2s;
}

.edit-btn:hover {
  background: rgba(0, 0, 0, 0.9);
}

/* 添加链接按钮 */
.add-link {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.add-btn {
  font-size: 1.5em;
  background: none;
  border: none;
  color: white;
  cursor: pointer;
  transition: transform 0.2s ease;
}

.add-btn:hover {
  transform: scale(1.2);
}

/* 模态框样式 */
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
  z-index: 999;
  text-align: center;
}

.modal-content {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.modal-content label {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.modal-content input {
  width: 100%;
  padding: 8px;
  margin-top: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.modal-actions {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-top: 15px;
}

.confirm-btn {
  background: #4caf50;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

.confirm-btn:hover {
  background: #45a049;
}

.cancel-btn {
  background: #f44336;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

.cancel-btn:hover {
  background: #d32f2f;
}

.delete-btn {
  background: #d9534f;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

.delete-btn:hover {
  background: #c9302c;
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
