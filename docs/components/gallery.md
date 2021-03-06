## Gallery
画廊

| 参数 | 类型 | 描述 | 默认值 |
| --- | --- | --- | --- |
| options | <code>object</code> | 配置项 | - |
| options.indicatorDots | <code>boolean</code> | 是否显示面板指示点 | false |
| options.indicatorColor | <code>string</code> | 指示点颜色 | rgba(0, 0, 0, .3) |
| options.indicatorActiveColor | <code>string</code> | 当前选中的指示点颜色 | #000000 |
| options.autoplay | <code>boolean</code> | 是否自动切换 | false |
| options.interval | <code>number</code> | 自动切换时间间隔 | 5000 |
| options.duration | <code>number</code> | 滑动动画时长 | 500 |
| options.circular | <code>boolean</code> | 是否采用衔接滑动 | false |
| options.vertical | <code>boolean</code> | 滑动方向是否为纵向 | false |
| options.showDelete | <code>boolean</code> | 是否显示删除按钮 | true |
| options.current | <code>number</code> | 当前显示图片的索引值 | 0 |
| options.urls | <code>array</code> | 需要预览的图片链接列表 | [] |
| options.delete | <code>function</code> | 点击删除的回调函数 | - |
| options.cancel | <code>function</code> | 点击关闭的回调函数 | - |
| options.onTap | <code>function</code> | 图片点击事件 | - |

**Example**  
```html
<wux-gallery id="wux-gallery" />

<view class="page">
    <view class="page__hd">
        <view class="page__title">Gallery</view>
        <view class="page__desc">画廊</view>
    </view>
    <view class="page__bd">
        <view class="weui-cells__title">基于小程序原生的wx.previewImage</view>
        <view class="weui-cells weui-cells_after-title">
            <view class="weui-cell">
                <view class="weui-cell__bd">
                    <view class="weui-uploader">
                        <view class="weui-uploader__bd">
                            <view class="weui-uploader__files">
                                <block wx:for-items="{{ urls }}" wx:key="{{ index }}">
                                    <view class="weui-uploader__file" bindtap="previewImage" data-current="{{ item }}">
                                        <image class="weui-uploader__img" src="{{ item }}" />
                                    </view>
                                </block>
                            </view>
                        </view>
                    </view>
                </view>
            </view>
        </view>
        <view class="weui-cells__title">自定义gallery</view>
        <view class="weui-cells weui-cells_after-title">
            <view class="weui-cell">
                <view class="weui-cell__bd">
                    <view class="weui-uploader">
                        <view class="weui-uploader__bd">
                            <view class="weui-uploader__files">
                                <block wx:for-items="{{ urls }}" wx:key="{{ index }}">
                                    <view class="weui-uploader__file" bindtap="showGallery" data-current="{{ index }}">
                                        <image class="weui-uploader__img" src="{{ item }}" />
                                    </view>
                                </block>
                            </view>
                        </view>
                    </view>
                </view>
            </view>
        </view>
        <view class="weui-cells__title">带说明文案</view>
        <view class="weui-cells weui-cells_after-title">
            <view class="weui-cell">
                <view class="weui-cell__bd">
                    <view class="weui-uploader">
                        <view class="weui-uploader__bd">
                            <view class="weui-uploader__files">
                                <block wx:for-items="{{ urls }}" wx:key="{{ index }}">
                                    <view class="weui-uploader__file" bindtap="showGallery2" data-current="{{ index }}">
                                        <image class="weui-uploader__img" src="{{ item }}" />
                                    </view>
                                </block>
                            </view>
                        </view>
                    </view>
                </view>
            </view>
        </view>
    </view>
</view>
```

```js
import { $wuxGallery } from '../../dist/base/index'

Page({
    data: {
        urls: [
            'https://unsplash.it/200/200',
            'https://unsplash.it/300/300',
            'https://unsplash.it/400/400',
            'https://unsplash.it/600/600',
            'https://unsplash.it/800/800',
            'https://unsplash.it/900/900',
            'https://unsplash.it/1000/1000',
            'https://unsplash.it/1200/1200',
        ],
    },
    onLoad() {},
    showGallery(e) {
        const { current } = e.currentTarget.dataset
        const { urls } = this.data

        this.$wuxGallery = $wuxGallery()

        this.$wuxGallery.show({
            current,
            urls,
            [`delete`]: (current, urls) => {
                urls.splice(current, 1)
                this.setData({
                    urls,
                })
                return true
            },
            cancel() {
                console.log('Close gallery')
            },
            onTap(current, urls) {
                console.log(current, urls)
                return true
            },
            onChange(e) {
                console.log(e)
            },
        })
    },
    showGallery2(e) {
        const { current } = e.currentTarget.dataset
        const { urls } = this.data

        $wuxGallery().show({
            current,
            urls: urls.map((n) => ({ image: n, remark: n })),
            showDelete: false,
            indicatorDots: true,
            indicatorColor: '#fff',
            indicatorActiveColor: '#04BE02',
        })
    },
    previewImage(e) {
        const { current } = e.currentTarget.dataset
        const { urls } = this.data

        wx.previewImage({
            current,
            urls,
        })
    },
})
```