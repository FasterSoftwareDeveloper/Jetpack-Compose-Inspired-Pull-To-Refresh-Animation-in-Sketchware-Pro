# Jetpack Compose Inspired Pull-To-Refresh Animation in Sketchware Pro

[![Pull to Refresh Demo](https://raw.githubusercontent.com/FasterSoftwareDeveloper/Jetpack-Compose-Inspired-Pull-To-Refresh-Animation-in-Sketchware-Pro/refs/heads/main/thumbnail.png)](https://youtu.be/StO4e8eazdA)

This project provides a custom Android `RelativeLayout` that mimics the fluid, physics-based pull-to-refresh animation found in Jetpack Compose's `pullRefresh` modifier. It's built for traditional XML layouts and is perfect for easy integration into **Sketchware Pro** projects, giving them a modern Material 3 feel.

The layout uses a custom `MaterialCardView` as the refresh indicator and applies physics-based "rubber-banding" to the content as you pull.

---

## ✨ Features

* **Material 3 Design:** Uses a `MaterialCardView` for the indicator, fitting perfectly with modern M3 themes.
* **Physics-Based Rubber-Banding:** The main content (your `RecyclerView` or `ScrollView`) stretches with a "tightness" factor as you pull, and dampens its movement as it reaches a max distance, creating a realistic elastic feel.
* **Dynamic Indicator Animation:** The indicator card scales and fades in with a bouncy `OvershootInterpolator` as you pull.
* **Velocity-Based Icon Rotation:** The icon *inside* the indicator card rotates based on the **speed and direction** of your pull, not just the distance, for a dynamic, responsive effect.
* **Smooth Animation Transitions:**
    * **Rotation Decay:** When you release, the icon's rotation doesn't stop abruptly but smoothly decays to a stop.
    * **Bounce on Refresh:** When a refresh is triggered, the content animates back to its position with a satisfying `OvershootInterpolator` bounce.
    * **Gentle Cancel:** If you release before triggering a refresh, all elements animate smoothly back to their starting positions.
* **State-Aware:**
    * Correctly manages `isRefreshing` state.
    * Can be controlled programmatically using `setRefreshing(true)` and `setRefreshing(false)`.
* **Fully Customizable via XML:** You can control the animation physics directly from your XML layout:
    * `ptr_triggerDistance`: How far the user must pull to trigger a refresh.
    * `ptr_contentDownMaxDistance`: The maximum distance the content can be pulled down.
    * `ptr_contentDownTightness`: The "tightness" of the rubber-band effect for the content (0.1 - 1.0).
    * `ptr_indicatorDownTightness`: The "tightness" for the indicator, allowing for parallax effects.
    * `ptr_indicatorGroupMargin`: The top margin for the indicator.
* **Efficient:** Only intercepts touch events when the child view (`RecyclerView`, etc.) is scrolled to the top (`canChildScrollUp() == false`), ensuring it doesn't interfere with normal scrolling.

---

## 🚀 How to Use in Sketchware Pro

### 1. Add the Java Code
Create a new Java file in Sketchware Pro (e.g., `FasterPullToRefreshLayout.java`) and paste the entire `FasterPullToRefreshLayout.java` code.

**IMPORTANT:** You *must* change the package name at the top of the file to match your project's package name.
`Package yt.m3pulltorefresh.faster;` ➡️ `Package com.my.package.name;`

### 2. Add Required Resources
This layout requires several additional resource files to function. You must add these to your project:

* **Layouts (`res/layout/`):**
    * `pull_indicator.xml` (Contains the `LoadingIndicator`)
    * `custom_pull_indicator.xml` (Contains the `MaterialCardView` and `AppCompatImageView`)
* **Drawable (`res/drawable/`):**
    * `twelve_styled.xml` (This is the vector drawable that rotates. You can replace this with your own icon).
* **Attributes (`res/values/`):**
    * `attrs.xml` (This defines the custom XML attributes like `ptr_triggerDistance`).

### 3. Use in your XML Layout
In your activity's XML, wrap your main scrollable content (like a `RecyclerView` or `ScrollView`) with the `FasterPullToRefreshLayout`.

> **Note:** Make sure to use your full package name in the XML tag.

```xml
<com.my.package.name.FasterPullToRefreshLayout
    xmlns:android="[http://schemas.android.com/apk/res/android](http://schemas.android.com/apk/res/android)"
    xmlns:app="[http://schemas.android.com/apk/res-auto](http://schemas.android.com/apk/res-auto)"
    android:id="@+id/pull_to_refresh"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    
    // --- Optional Customization ---
    app:ptr_triggerDistance="100dp"
    app:ptr_contentDownMaxDistance="220dp"
    app:ptr_contentDownTightness="0.45"
    app:ptr_indicatorDownTightness="0.12"
    app:ptr_indicatorGroupMargin="20dp">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/my_recycler_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</com.my.package.name.FasterPullToRefreshLayout>
```
4. Set up the Listener in Java
In your onCreate method, get a reference to the layout and set the refresh listener.
```java
// Import your layout
import com.my.package.name.FasterPullToRefreshLayout;

// ... inside onCreate ...

final FasterPullToRefreshLayout ptrLayout = findViewById(R.id.pull_to_refresh);

ptrLayout.setOnRefreshListener(new FasterPullToRefreshLayout.OnRefreshListener() {
    @Override
    public void onRefresh() {
        // --- YOUR REFRESH LOGIC HERE ---
        // (e.g., make a network request, load from database)
        
        // After your data is loaded, you MUST call
        // setRefreshing(false) to hide the indicator.
        
        // Example: Post a delayed event to simulate a network call
        new android.os.Handler(android.os.Looper.getMainLooper()).postDelayed(
            new Runnable() {
                @Override
                public void run() {
                    // Data is "loaded"
                    ptrLayout.setRefreshing(false);
                }
            }, 3000 // 3-second delay
        );
    }
});
```

## License

- **Open & Free (No Restrictions):**  
  This project is released as **CC0 1.0 Universal**. You are free to use, modify, and share it for any purpose, including commercial projects—no attribution required, no restrictions.

- **Third-Party Libraries:**  
  This project may include third-party libraries. Each library is subject to its own license. Please review their licenses if you plan to use them.
