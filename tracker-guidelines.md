# 🧾 投保系统行为埋点文档 v1.0

> 用于规范化前端用户行为追踪事件，统一 `event_type` 命名和 `event_detail` 字段结构，确保行为数据清晰、可分析。

## 📋 埋点事件一览表

| 编号 | 场景描述             | `event_type`           | `event_detail` 字段结构                         | 使用示例                                                                                                             |
|------|----------------------|------------------------|--------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| 1 | 页面导航             | `page_navigate`        | `{ to, component }`                              | *自动注入*                                                                                                           |
| 2 | 页面隐藏（切出 Tab） | `page_hidden`          | `{ url }`                                        | *自动注入*                                                                                                           |
| 3 | 页面激活（切回 Tab） | `page_visible`         | `{ url }`                                        | *自动注入*                                                                                                           |
| 4 | 普通按钮点击         | `dom_btn_click`        | `{ name, page, meta? }`                          | `v-track:click="{ name: 'submit_step1' }"`                                                                       |
| 5 | 图标点击             | `dom_icon_click`       | `{ name, page }`                                 | `v-track:click="{ event_type: 'dom_icon_click', name: 'close_modal' }"`                                          |
| 6 | 表单按钮提交         | `dom_form_click`       | `{ name, page }`                                 | `v-track:click="{ event_type: 'dom_form_click', name: 'submit_enrollment' }"`                                    |
| 7 | 文本输入框失焦       | `dom_input_blur`       | `{ field, value, page }`                         | `v-track:blur="{ name: 'input_email', meta: { field: 'email', value: formData.email } }"`                        |
| 8 | 下拉框选择更改       | `dom_select_change`    | `{ field, value, page }`                         | `v-track:change="{ name: 'select_school', meta: { field: 'school', value: formData.school } }"`                  |
| 9 | 单选项（radio）点击  | `dom_radio_select`     | `{ field, value, page }`                         | `v-track:click="{ name: 'select_plan', meta: { field: 'plan', value: 'premium' } }"`                             |
| 10 | 多选项（checkbox）切换 | `dom_checkbox_toggle`  | `{ field, value, page }`                         | `v-track:change="{ name: 'agree_terms', meta: { field: 'terms', value: true } }"`                                |
| 11 | 步骤切换行为       | `step_navigate`        | `{ from_step, to_step, page }`                   | `v-track:click="{ event_type: 'step_navigate', name: 'step1_to_step2', meta: { from_step: '1', to_step: '2' } }"` |
| 12 | 弹窗关闭           | `dom_modal_close`      | `{ name, page }`                                 | `v-track:click="{ event_type: 'dom_modal_close', name: 'close_terms_popup' }"`                                   |
| 13 | 表单报错提示       | `error_show`           | `{ error_code, message, page }`                  | `this.$tracker.trackEvent('error_show', { error_code: 'E001', message: 'Invalid phone' })`                       |
| 14 | 提交成功提示       | `form_submit_success`  | `{ page, submission_id }`                        | `this.$tracker.trackEvent('form_submit_success', { submission_id: 'abc123' })`                                   |

## 🔧 使用说明

- 所有事件都通过 `this.$tracker.trackEvent(event_type, event_detail)` 上报。
- 推荐统一使用封装好的 `v-track` 指令，支持 `click` / `blur` / `change` 等 DOM 行为监听。
- `page` 字段由指令或 Tracker 内部自动注入，不需要手动指定。
- 所有 `event_type` 命名使用蛇形（下划线）风格，便于 SQL 分析。

## 📌 命名规则建议

| 类型             | 命名前缀           | 示例               |
|------------------|--------------------|--------------------|
| 页面行为         | `page_`            | `page_navigate`    |
| DOM 操作行为     | `dom_`             | `dom_btn_click`    |
| 业务逻辑行为     | `form_` / `step_`  | `form_submit_success`, `step_navigate` |
| 异常提示行为     | `error_`           | `error_show`       |

## ✅ 当前目录结构

```
resources/js/
│
├── directives/
│   └── track.js             // 自定义 v-track 指令
│
├── utils/
│   └── Tracker.js           // 核心行为追踪类
```
