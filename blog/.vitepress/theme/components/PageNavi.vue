<template>
  <!-- 自定义主菜单。侧栏和首页顶栏菜单 -->
  <div class="mainnavi top" v-if="props.type === 'top'">
    <ul class="ul">
      <li class="li pc">
        <VDropdown :distance="6" :placement="'bottom'">
          <button class="tags a"><svg class="svg" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
              <path
                d="M10.9042 2.10025L20.8037 3.51446L22.2179 13.414L13.0255 22.6063C12.635 22.9969 12.0019 22.9969 11.6113 22.6063L1.71184 12.7069C1.32131 12.3163 1.32131 11.6832 1.71184 11.2926L10.9042 2.10025ZM13.7327 10.5855C14.5137 11.3666 15.78 11.3666 16.5611 10.5855C17.3421 9.80448 17.3421 8.53815 16.5611 7.7571C15.78 6.97606 14.5137 6.97606 13.7327 7.7571C12.9516 8.53815 12.9516 9.80448 13.7327 10.5855Z"
                fill="currentColor"></path>
            </svg>标签</button>
          <template #popper>
            <Tags />
          </template>
        </VDropdown>
      </li>
      <li class="li pc">
        <VDropdown :distance="6" :placement="'bottom'">
          <button class="archives a"><svg class="svg" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
              <path
                d="M3 10H21V20.0044C21 20.5543 20.5551 21 20.0066 21H3.9934C3.44476 21 3 20.5552 3 20.0044V10ZM9 12V14H15V12H9ZM2 3.99981C2 3.44763 2.45531 3 2.9918 3H21.0082C21.556 3 22 3.44372 22 3.99981V8H2V3.99981Z"
                fill="currentColor"></path>
            </svg>存档</button>
          <template #popper>
            <Archives />
          </template>
        </VDropdown>
      </li>
      <li class="li h5" @click="choose('tags')"
        :class="{ on: (page.relativePath === 'pages/tags.md' && !currentCategory && !currentTag) || selected === 'tags' }">
        <a class="a" :href="withBase(`/pages/tags`)"><svg class="svg" xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24">
            <path
              d="M10.9042 2.10025L20.8037 3.51446L22.2179 13.414L13.0255 22.6063C12.635 22.9969 12.0019 22.9969 11.6113 22.6063L1.71184 12.7069C1.32131 12.3163 1.32131 11.6832 1.71184 11.2926L10.9042 2.10025ZM13.7327 10.5855C14.5137 11.3666 15.78 11.3666 16.5611 10.5855C17.3421 9.80448 17.3421 8.53815 16.5611 7.7571C15.78 6.97606 14.5137 6.97606 13.7327 7.7571C12.9516 8.53815 12.9516 9.80448 13.7327 10.5855Z"
              fill="currentColor"></path>
          </svg>标签</a>
      </li>
      <li class="li h5" @click="choose('archives')"
        :class="{ on: (page.relativePath === 'pages/archives.md' && !currentCategory && !currentTag) || selected === 'archives' }"><a
          class="a" :href="withBase(`/pages/archives`)"><svg class="svg" xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24">
            <path
              d="M3 10H21V20.0044C21 20.5543 20.5551 21 20.0066 21H3.9934C3.44476 21 3 20.5552 3 20.0044V10ZM9 12V14H15V12H9ZM2 3.99981C2 3.44763 2.45531 3 2.9918 3H21.0082C21.556 3 22 3.44372 22 3.99981V8H2V3.99981Z"
              fill="currentColor"></path>
          </svg>存档</a></li>
    </ul>
  </div>
  <!-- 左侧边栏菜单 -->
  <div class="mainnavi left" v-else>
    <ul class="ul">
      <li class="li" @click="choose('home')"
        :class="{ on: (page.relativePath === 'index.md' && !currentCategory && !currentTag && selected === '') || selected === 'home' }">
        <a class="a" :href="withBase(`/`)">
          首页
        </a>
      </li>
      <li class="li" @click="choose(key.toString())"
        :class="{ on: matchesQueryParams(key.toString()) || selected === key.toString() }"
        v-for="(item, key) in data">
        <a class="a" :href="withBase(`/?category=${key.toString()}`)">
          {{ key }}<strong class="VPBadge tip strong mini">{{ data[key].length }}</strong>
        </a>
      </li>
    </ul>
    
    <!-- 动态渲染导航分组 -->
    <template v-for="(group, index) in navGroups" :key="index">
      <h4 class="h4" v-if="group.text">
        <!-- <svg class="svg" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
          <path d="M22 20V7L20 3H4L2 7.00353V20C2 20.5523 2.44772 21 3 21H21C21.5523 21 22 20.5523 22 20ZM4 9H20V19H4V9ZM5.236 5H18.764L19.764 7H4.237L5.236 5ZM15 11H9V13H15V11Z" fill="currentColor"></path>
        </svg> -->
        {{ group.text }}
      </h4>
      <ul class="ul">
        <li class="li" v-for="item in group.items" :key="item.key" @click="choose(item.key)"
            :class="{ on: isCurrentPage(item.path) || matchesQueryParams(item.key, item.path) || selected === item.key }">
          <a class="a" :href="withBase(item.path)">
            <span class="icon-wrapper" v-if="item.svg" v-html="processSvg(item.svg)"></span>
            <svg class="svg" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" v-html="item.icon" v-else></svg>
            {{ item.text }}
          </a>
        </li>
      </ul>
    </template>
  </div>
</template>

<script lang="js" setup>
import { computed, ref } from 'vue'
import { useData, withBase } from 'vitepress'
import { initCats } from '../functions'
import { data as themeposts } from '../posts.data'

let url = window.location.href.split('?')[1]
let params = new URLSearchParams(url || '')
const currentCategory = params.get('category')
const currentTag = params.get('tag')
const { page, theme } = useData()
const data = computed(() => initCats(themeposts))
const navGroups = computed(() => theme.value.navGroups || [])

const props = defineProps({
  type: {
    type: String,
    default: 'left'
  }
})
const selected = ref('')
const choose = (e) => {
  selected.value = e
}

// 处理完整SVG，添加fill="currentColor"
const processSvg = (svgString) => {
  if (!svgString) return ''
  
  // 如果已经有fill属性，但不是currentColor，替换它
  if (svgString.includes('fill="') && !svgString.includes('fill="currentColor"')) {
    return svgString.replace(/fill="[^"]+"/, 'fill="currentColor"')
  }
  
  // 对于没有fill属性的path添加fill="currentColor"
  if (svgString.includes('<path') && !svgString.includes('fill=')) {
    return svgString.replace(/<path/g, '<path fill="currentColor"')
  }
  
  return svgString
}

// 判断导航项是否匹配当前URL参数
const matchesQueryParams = (navKey, navPath) => {
  // 如果navPath包含参数，检查是否匹配
  if (navPath && navPath.includes('?')) {
    const targetParams = new URLSearchParams(navPath.split('?')[1])
    const targetCategory = targetParams.get('category')
    const targetTag = targetParams.get('tag')
    
    if (targetCategory && currentCategory === targetCategory) return true
    if (targetTag && currentTag === targetTag) return true
  }
  
  // 检查navKey是否直接匹配当前category或tag
  if (navKey && (navKey === currentCategory || navKey === currentTag)) return true
  
  return false
}

// 判断是否是当前页面
const isCurrentPage = (path) => {
  if (!path) return false
  
  // 检查目标路径中的参数
  if (path.includes('?')) {
    const targetParams = new URLSearchParams(path.split('?')[1])
    const targetCategory = targetParams.get('category')
    const targetTag = targetParams.get('tag')
    
    // 如果存在类别参数，检查是否匹配
    if (targetCategory && currentCategory === targetCategory) {
      return true
    }
    
    // 如果存在标签参数，检查是否匹配
    if (targetTag && currentTag === targetTag) {
      return true
    }
    
    // 参数不匹配
    return false
  }
  
  // 没有参数的普通路径匹配
  const currentPath = window.location.pathname.endsWith('/') 
    ? window.location.pathname.slice(0, -1) 
    : window.location.pathname
  
  const targetPath = path.split('?')[0]
  if (currentPath === targetPath || currentPath + '/' === targetPath) {
    // 确保没有category或tag参数时才高亮普通路径
    return !currentCategory && !currentTag
  }
  
  return false
}
</script>

<style scoped>
.h4 {
  margin: 0;
  padding: 0;
  font-weight: bold;
  font-size: 12px;
  margin-top: 20px;
  display: flex;
  flex-direction: row;
  align-items: center;

  .svg {
    width: 14px;
    height: 14px;
    margin-right: 5px;
    fill: currentColor;
    display: inline-block;
  }
}

.tags,
.archives {
  padding: 0;
  /* color: #ffffff; */
  display: flex;
  flex-direction: row;
  align-items: center;
  height: 44px;
  line-height: 1;
  transform: translateX(0px);
  transition: .3s;
}

.mainnavi.left {
  position: relative;
  margin-top: 20px;
  /* background-color: var(--vp-c-gray-1); */
  width: 80%;
  border-radius: 16px;
  font-size: 14px;
  z-index: 1000;
  min-height: calc(100vh - 200px);

  .ul {

    display: flex;
    flex-direction: column;
    padding: 10px 0;

    .li {
      /* padding: 0 10px; */
      /* margin: 0 10px; */
      /* border-bottom: 1px solid var(--vp-c-divider); */

      .a {
        padding: 0;
        /* color: #ffffff; */
        display: flex;
        flex-direction: row;
        align-items: center;
        height: 44px;
        font-size: 14px;
        line-height: 1;
        transform: translateX(0px);
        transition: .3s;
        width: 100%;
        cursor: pointer;
      }

      .svg {
        width: 16px;
        height: 16px;
        margin-right: 5px;
        fill: currentColor;
      }

      .icon-wrapper {
        display: inline-flex;
        margin-right: 5px;
        
        :deep(svg) {
          width: 16px;
          height: 16px;
          fill: currentColor;
        }
      }

      .a:hover {
        /* color: #ffffff; */
        transform: translateX(10px);
        transition: .3s;
      }
    }

    .li.pc {
      display: none;
    }

    .li.h5 {
      display: block;
    }

    .li:last-child {
      /* border-bottom: none; */
    }

    .li.on .a {
      /* font-weight: bold; */
      color: var(--vp-c-brand);
    }
  }
}

.mainnavi.top {
  margin-bottom: 32px;
  /* background-color: var(--vp-c-gray-1); */
  width: 100%;
  border-radius: 16px;
  font-size: 14px;

  display: flex;
  flex-direction: row;
  justify-content: center;

  .ul {

    display: flex;
    flex-direction: row;
    padding: 10px 0;

    .li {
      padding: 0 10px;
      margin: 0 10px;
      /* border-bottom: 1px solid var(--vp-c-divider); */

      .a {
        padding: 0;
        /* color: #ffffff; */
        display: flex;
        flex-direction: row;
        align-items: center;
        height: 44px;
        font-size: 14px;
        line-height: 1;
        transform: translateX(0px);
        transition: .3s;
        width: 100%;
      }

      .svg {
        width: 16px;
        height: 16px;
        margin-right: 5px;
        fill: currentColor
      }

      .a:hover {
        /* color: #ffffff; */
        /* transform: translateX(10px); */
        transition: .3s;
        color: var(--vp-c-brand);
      }
    }

    .li.pc {
      display: none;
    }

    .li.h5 {
      display: block;

      background: var(--vp-c-bg-alt);
      border-radius: 40px;
      padding: 0 20px;
    }

    .li:last-child {
      border-bottom: none;
    }

    .li.on .a {
      font-weight: bold;
      color: var(--vp-c-brand);
    }
  }
}

@media (min-width: 960px) {

  .mainnavi.top,
  .mainnavi.left {

    .ul {
      .li.pc {
        display: block;

        background: var(--vp-c-bg-alt);
        border-radius: 40px;
        padding: 0 20px;
      }

      .li.h5 {
        display: none;
      }
    }
  }
}
</style>