<template>
  <div>
    <div class="head">
      <div class="main">
        <div class="left">
          <div class="logo" key="logo" id="logo">
            <router-link to="/">
              <img src="../../assets/logo.png" alt="logo" />
            </router-link>
            <div class="left_title">
              <span class="main_title">深圳市人大议案建议管理综合平台</span>
              <span class="small_title">人大管理系统</span>
            </div>
          </div>
          <!-- <div style="width:100%"></div> -->
        </div>
        <div class="right">
          <img src="../../assets/head_bg_img.png" class="bgImg" alt="bg"/>
          <div class="toolbar">
            <div class="toolbarItem toolbarLine">
              <a-avatar class="avatar" src="http://192.168.1.160:8002/static/default_avatars.37c79449.png" alt="avatar" />
              <span class="toolbarItemText">管理员</span>
            </div>
            <div class="toolbarItem toolbarLine" @click="setting">
              <a-icon class="toolbarItemIcon" type="setting" theme="filled" />
              <span class="toolbarItemText">设置</span>
            </div>
            <div class="toolbarItem">
              <a-icon class="toolbarItemIcon" type="logout" @click="logout" />
              <span class="toolbarItemText">退出</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import {Modal, Avatar, Icon} from 'antd'

export default {
  name: 'Header',
  data () {
    return {}
  },
  methods: {

  }
}
</script>

<style lang="less" scoped>
@import '../index.less';

@pro-header-hover-bg: rgba(0, 0, 0, 0.025);
:global {
  .ant-dropdown-menu-item .anticon {
    padding-right: 5px;
  }
}

.head {
  height: 75px;
  padding: 0;
  // background: #fff;
  background-image: url(/assets/head_bg.png);
  background-color: rgb(232, 250, 250);
  border: none;
  box-shadow: 0 1px 4px rgba(0, 21, 41, 0.08);
  position: relative;
  z-index: 11;

  .main {
    display: flex;
    height: 75px;
    padding-left: 24px;

    &.wide {
      max-width: 1200px;
      margin: auto;
      padding-left: 0;
    }

    .left {
      display: flex;
      flex: 1;
      z-index: 1;
    }

    .right {
      display: flex;
      float: right;
      height: 100%;
      // overflow: hidden;

      .bgImg {
        position: absolute;
        top: 0;
        right: 280px;
      }

      .toolbar {
        z-index: 10;
        display: flex;
        background: rgba(245, 245, 245, 0.5);

        .toolbarLine {
          border-right: 1px solid #fff;
        }

        .toolbarItem {
          display: flex;
          flex-direction: column;
          align-items: center;
          padding: 0 22px;
          cursor: default;

          .avatar {
            margin-top: 12px;
            background: #FFF;
          }

          .toolbarItemText {
            font-weight: 600;
          }

          .toolbarItemIcon {
            margin-top: 12px;
            color: rgba(0, 0, 0, 0.45);
            font-size: 32px;
          }
        }
      }

      .action {
        cursor: pointer;
        padding: 0 12px;
        display: flex;
        align-items: center;
        transition: all 0.3s;
        height: 100%;

        > i {
          vertical-align: middle;
          color: @text-color;
        }

        &:hover {
          background: @pro-header-hover-bg;
        }

        :global(&.ant-popover-open) {
          background: @pro-header-hover-bg;
        }
      }

      .account {
        .avatar {
          margin: 20px 8px 20px 0;
          color: @primary-color;
          background: rgba(255, 255, 255, 0.85);
          width: 36px;
          height: 36px;
          line-height: 36px;
          border-radius: 50%;
        }
      }

      .name {
        white-space: nowrap;
      }
    }
  }
}

.logo {
  display: flex;
  height: 75px;
  align-items: center;
  transition: all 0.3s;
  overflow: hidden;

  img {
    display: inline-block;
    vertical-align: middle;
    height: 60px;
  }

  .left_title {
    display: flex;
    flex-direction: column;
    margin-left: 15px;

    .main_title {
      color: rgba(0, 0, 0, 0.65);
      font-size: 22px;
      font-weight: 600;
      white-space: nowrap;
    }

    .small_title {
      color: rgba(0, 0, 0, 0.65);
      font-size: 16px;
      font-weight: 600;
      white-space: nowrap;
    }
  }

  .leftOther {
    display: flex;
    margin-left: 30px;
    align-items: center;

    .leftTitle {
      display: flex;
      flex-direction: column;
      line-height: 28px;
      height: 75px;
      margin: 8px;
      white-space: nowrap;

      .lgTitle {
        font-size: 22px;
        font-weight: 600;
      }

      .smTitle {
        font-size: 14px;
        font-weight: 600;
      }

      .topTitle {
        font-size: 14px;
        color: #555555;
      }

      .subTitle {
        font-size: 22px;
        color: #555555;
        text-indent: 1em;
      }
    }

    .leftLine {
      width: 1px;
      background: #ccc;
      height: 50px;
      margin: 10px;
    }
  }
}

.cw_topheader_modal_body {
  display: flex;
  justify-content: space-between;
  margin: 0 48px;
}

@media screen and (max-width: @screen-xs) {
  .head {
    .main {
      padding-left: 5px;

      .left {
        .logo {
          img {
            height: 50px;
          }
        }

        .left_title {
          margin-left: 8px;

          .main_title {
            font-size: 16px;
          }

          .small_title {
            font-size: 14px;
          }
        }
      }

      .right {
        .noticeImg {
          display: none;
        }

        .bgImg {
          right: 0;
          opacity: 0.65;
          filter: alpha(opacity=65);
        }

        .toolbar {
          .toolbarItem {
            padding: 0;

            .avatar {
              display: none;
            }

            .toolbarItemText {
              display: none;
              font-weight: 600;
            }

            .toolbarItemIcon {
              margin-top: 30px;
              padding: 0 8px;
              font-size: 20px;
            }
          }
        }

        .name {
          font-size: 14px;
          white-space: nowrap;
          overflow: hidden;
          text-overflow: ellipsis;
          width: 70px;
        }
      }
    }
  }

  .cw_topheader_modal_body {
    flex-direction: column;

    button {
      margin-bottom: 15px;
    }
  }
}

</style>
