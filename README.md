# ShimmerRecyclerView

[![Build Status](https://travis-ci.com/omtodkar/ShimmerRecyclerView.svg)](https://travis-ci.com/omtodkar/ShimmerRecyclerView) [![Download](https://api.bintray.com/packages/todkars/android/shimmer-recyclerview/images/download.svg?version=0.4.0)](https://bintray.com/todkars/android/shimmer-recyclerview/0.4.0/link) ![GitHub](https://img.shields.io/github/license/omtodkar/ShimmerRecyclerView.svg?label=License) [![API](https://img.shields.io/badge/API-14%2B-brightgreen.svg?style=flat)](https://android-arsenal.com/api?level=14) [![Android Arsenal]( https://img.shields.io/badge/Android%20Arsenal-ShimmerRecyclerView-green.svg?style=flat )]( https://android-arsenal.com/details/1/7612 ) 

ShimmerRecyclerView is an custom RecyclerView library based on Facebook's [Shimmer](https://github.com/facebook/shimmer-android) effect for Android library and inspired from [Sharish's ShimmerRecyclerView](https://github.com/sharish/ShimmerRecyclerView).

There is reason behind creating a separate library for ShimmerRecyclerView, most of libraries doesn't not support runtime switching of `LayoutManager` or shimmer `layout` resources. Secondly the other similar library is build upon [Supercharge's ShimmerLayout](https://github.com/team-supercharge/ShimmerLayout) which is I feel very less active in terms of release. So I came up with an alternative.

## Demo

Change layout manager in runtime `LinearLayoutManager` to `GridLayoutManager` and Shimmer will adopt accordingly.
 Setup `mShimmerRecyclerView.setItemViewType(ShimmerAdapter.ItemViewType)` to change view type of Shimmer adapter.

|        Switching Layouts     |        Multiple Type Demo     |        Grid Demo             |
| ---------------------------- | ----------------------------- | ---------------------------- |
| <img src='demo/list-grid-demo.gif' height=480 width=240 /> | <img src='demo/list-demo.gif' height=480 width=240 /> | <img src='demo/grid-demo.gif' height=480 width=240 /> |

## Download

To include `ShimmerRecyclerView` in your project, add the following to your dependencies:

**app/build.gradle**
```groovy
dependencies {
   implementation 'com.facebook.shimmer:shimmer:0.5.0'
   implementation 'com.todkars:shimmer-recyclerview:0.4.1'
}
```

## Usage
The following snippet shows how you can use Shimmer RecyclerView in your project.

**Layout** 

```xml
<com.todkars.shimmer.ShimmerRecyclerView
    android:id="@+id/shimmer_recycler_view"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"
    app:shimmer_recycler_layout="@layout/list_item_shimmer_layout"
    app:shimmer_recycler_item_count="10" />
```

**Activity**

```java
public class MainActivity extends Activity {
    
    private ShimmerRecyclerView mShimmerRecyclerView;
    
    //... other variables
    
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        
        mShimmerRecyclerView = findViewById(R.id.shimmer_recycler_view);
        
        mShimmerRecyclerView.setLayoutManager(new LinearLayoutManager(this),
            R.layout.list_item_shimmer_layout);
        
        mShimmerRecyclerView.setAdapter(adapter);
        
        // This is optional, use if no attributes are mentioned in layout xml resource.
        // WARNING: Setting Shimmer programmatically will obsolete all shimmer attributes.
        /* mShimmerRecyclerView.setShimmer(mShimmer); */
        
        /* Shimmer layout view type depending on List / Gird */
        mShimmerRecyclerView.setItemViewType((type, position) -> {
            switch (type) {
                case ShimmerRecyclerView.LAYOUT_GRID:
                    return position % 2 == 0
                            ? R.layout.list_item_shimmer_grid
                            : R.layout.list_item_shimmer_grid_alternate;

                default:
                case ShimmerRecyclerView.LAYOUT_LIST:
                    return position == 0 || position % 2 == 0
                            ? R.layout.list_item_shimmer
                            : R.layout.list_item_shimmer_alternate;
            }
        });
        
        mShimmerRecyclerView.showShimmer();     // to start showing shimmer
        
        // To stimulate long running work using android.os.Handler
        mHandler.postDelayed((Runnable) () -> {
            mShimmerRecyclerView.hideShimmer(); // to hide shimmer
        }, 3000);
    }
}
```

## Attributes

Most of the attributes used for **ShimmerRecyclerView** are same as Facebook's ***ShimmerFrameLayout*** below is detail table:

| Name | Attribute |  Description |
|---|---|---|
| Shimmer Layout | shimmer_recycler_layout | Layout reference used for as shimmer base. |
| Shimmer Item Count | shimmer_recycler_item_count | Number of shimmers to be shown in list, default is 9. |
| Clip to Children | `shimmer_recycler_clip_to_children` | Whether to clip the shimmering effect to the children, or to opaquely draw the shimmer on top of the children. Use this if your overall layout contains transparency. |
| Colored | `shimmer_recycler_colored` | Whether you want the shimmer to affect just the alpha or draw colors on-top of the children. |
| Base Color | `shimmer_recycler_base_color` | If colored is specified, the base color of the content. |
| Highlight Color | `shimmer_recycler_highlight_color` | If colored is specified, the shimmer's highlight color. |
| Base Alpha | `shimmer_recycler_base_alpha` | If colored is not specified, the alpha used to render the base view i.e. the unhighlighted view over which the highlight is drawn. |
| Highlight Alpha | `shimmer_recycler_highlight_alpha` | If colored is not specified, the alpha of the shimmer highlight. |
| Auto Start | `shimmer_recycler_auto_start` | Whether to automatically start the animation when the view is shown, or not. |
| Duration | `shimmer_recycler_duration` | Time it takes for the highlight to move from one end of the layout to the other. |
| Repeat Count | `shimmer_recycler_repeat_count` | Number of times of the current animation will repeat. |
| Repeat Delay | `shimmer_recycler_repeat_delay` | Delay after which the current animation will repeat. |
| Repeat Mode | `shimmer_recycler_repeat_mode` | What the animation should do after reaching the end, either restart from the beginning or reverse back towards it. |
| Direction | `shimmer_recycler_direction` |The travel direction of the shimmer highlight: left to right, top to bottom, right to left or bottom to top. |
| Dropoff | `shimmer_recycler_dropoff` | Controls the size of the fading edge of the highlight. |
| Intensity | `shimmer_recycler_intensity` | Controls the brightness of the highlight at the center |
| Shape | `shimmer_recycler_shape` | Shape of the highlight mask, either with a linear or a circular gradient. |
| Tilt | `shimmer_recycler_tilt` | Angle at which the highlight is tilted, measured in degrees. |
| Fixed Width or Height | `shimmer_recycler_fixed_width` or `shimmer_recycler_fixed_height` | Fixed sized highlight mask, if set, overrides the relative size value. |
| Width or Height Ratio | `shimmer_recycler_width_ratio` or `shimmer_recycler_height_ratio` | Size of the highlight mask, relative to the layout it is applied to. |

## [LICENSE](https://github.com/omtodkar/ShimmerRecyclerView/blob/master/LICENSE.md)

```text
    ShimmerRecyclerView a custom RecyclerView library
    Copyright (C) 2019  Omkar Todkar <omtodkar@gmail.com>

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see https://www.gnu.org/licenses/gpl.txt
```

Facebook Shimmer is published under [BSD License](https://github.com/facebook/shimmer-android/blob/master/LICENSE)
