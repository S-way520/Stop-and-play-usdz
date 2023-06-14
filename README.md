# Stop-and-play-usdz
RealityKit and SwiftUI and swift playground 
# The first part
![IMG_0101](https://github.com/S-way520/Stop-and-play-usdz/assets/95877651/4eb0a2bd-d1d2-430e-b8d2-625527c9c426)
# The second part
Code file 1
```swift
import SwiftUI
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```
Code file 2
```swift
import SwiftUI
import RealityKit
struct ContentView: View {
    @State var isPaused = true
    var body: some View {
        ZStack {
            ARViewContainer(isPaused: $isPaused)
                .ignoresSafeArea()
            VStack {
                Spacer()
                Button(isPaused ? "Stop" : "Play") {
                    self.isPaused.toggle()
                }
                .padding()
                .background(.red)
                .cornerRadius(10, antialiased: true)
                .padding()
            }
        }
    }
}
struct ARViewContainer: UIViewRepresentable {
    @Binding var isPaused: Bool
    @State var someAnimation: [AnimationPlaybackController] = []
    func makeUIView(context: Context) -> ARView {
        let arView = ARView(frame: .zero)
        let anchorEntity = AnchorEntity(plane: .horizontal)
         let modelEntity = try! ModelEntity.load(named: "toy_biplane_idle") 
        anchorEntity.addChild(modelEntity)
        arView.scene.addAnchor(anchorEntity)
        return arView
    }
    func updateUIView(_ uiView: ARView, context: Context) {
        if isPaused {
            uiView.scene.anchors[0].children[0].stopAllAnimations(recursive: true)
        } else {
            let modelEntity = try! ModelEntity.load(named: "toy_biplane_idle") 
            let someAnimation = modelEntity.availableAnimations[0].repeat(count: .max)//删除.repeat(count: .max);改 .max为3
            uiView.scene.anchors[0].children[0].playAnimation(someAnimation, transitionDuration: 1.0, startsPaused: false)
        }
    }
}
```
