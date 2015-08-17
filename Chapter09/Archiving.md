## 归档 - Archiving

苹果通过归档的方法来实现备忘录模式。它把对象转化成了流然后在不暴露内部属性的情况下存储数据。你可以读一读 《iOS 6 by Tutorials》 这本书的第 16 章，或者看下苹果的[归档和序列化文档](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Archiving/Archiving.html)。

### 如何使用归档

首先，我们需要让 `Album` 实现 `NSCoding` 协议，声明这个类是可被归档的。打开 `Album.swift` 在 `class` 那行后面加上 `NSCoding` ：

```swift
class Album: NSObject, NSCoding {
```

然后添加如下的两个方法：

```swift
required init(coder decoder: NSCoder) {
    super.init()
    self.title = decoder.decodeObjectForKey("title") as String?
    self.artist = decoder.decodeObjectForKey("artist") as String?
    self.genre = decoder.decodeObjectForKey("genre") as String?
    self.coverUrl = decoder.decodeObjectForKey("cover_url") as String?
    self.year = decoder.decodeObjectForKey("year") as String?
}

func encodeWithCoder(aCoder: NSCoder) {
    aCoder.encodeObject(title, forKey: "title")
    aCoder.encodeObject(artist, forKey: "artist")
    aCoder.encodeObject(genre, forKey: "genre")
    aCoder.encodeObject(coverUrl, forKey: "cover_url")
    aCoder.encodeObject(year, forKey: "year")
}
```

`encodeWithCoder` 方法是 `NSCoding` 的一部分，在被归档的时候调用。相对的， `init(coder:)` 方法则是用来解档的。很简单，很强大。

现在 `Album` 对象可以被归档了，添加一些代码来存储和加载 `Album` 数据。

在 `PersistencyManager.swift` 里添加如下代码：


```swift
func saveAlbums() {
    var filename = NSHomeDirectory().stringByAppendingString("/Documents/albums.bin")
    let data = NSKeyedArchiver.archivedDataWithRootObject(albums)
    data.writeToFile(filename, atomically: true)
} 
```

这个方法可以用来存储专辑。 `NSKeyedArchiver` 把专辑数组归档到了 `albums.bin` 这个文件里。

当我们归档一个包含子对象的对象时，系统会自动递归的归档子对象，然后是子对象的子对象，这样一层层递归下去。在我们的例子里，我们归档的是 `albums` 因为 `Array` 和 `Album` 都是实现 `NSCopying` 接口的，所以数组里的对象都可以自动归档。

用下面的代码取代 `PersistencyManager` 中的 `init` 方法：

```swift
override init() {
    super.init()
    if let data = NSData(contentsOfFile: NSHomeDirectory().stringByAppendingString("/Documents/albums.bin")) {
        let unarchiveAlbums = NSKeyedUnarchiver.unarchiveObjectWithData(data) as [Album]?
        if let unwrappedAlbum = unarchiveAlbums {
            albums = unwrappedAlbum
        }
    } else {
        createPlaceholderAlbum()
    }
}

func createPlaceholderAlbum() {
    //Dummy list of albums
    let album1 = Album(title: "Best of Bowie",
             artist: "David Bowie",
             genre: "Pop",
             coverUrl: "http://www.coversproject.com/static/thumbs/album/album_david%20bowie_best%20of%20bowie.png",
             year: "1992")

    let album2 = Album(title: "It's My Life",
           artist: "No Doubt",
           genre: "Pop",
           coverUrl: "http://www.coversproject.com/static/thumbs/album/album_no%20doubt_its%20my%20life%20%20bathwater.png",
           year: "2003")

    let album3 = Album(title: "Nothing Like The Sun",
               artist: "Sting",
           genre: "Pop",
           coverUrl: "http://www.coversproject.com/static/thumbs/album/album_sting_nothing%20like%20the%20sun.png",
           year: "1999")

    let album4 = Album(title: "Staring at the Sun",
           artist: "U2",
           genre: "Pop",
           coverUrl: "http://www.coversproject.com/static/thumbs/album/album_u2_staring%20at%20the%20sun.png",
           year: "2000")

    let album5 = Album(title: "American Pie",
           artist: "Madonna",
           genre: "Pop",
           coverUrl: "http://www.coversproject.com/static/thumbs/album/album_madonna_american%20pie.png",
           year: "2000")
    albums = [album1, album2, album3, album4, album5]
    saveAlbums()
}
```

我们把创建专辑数据的方法放到了 `createPlaceholderAlbum` 里，这样代码可读性更高。在新的代码里，如果存在归档文件， `NSKeyedUnarchiver` 从归档文件加载数据；否则就创建归档文件，这样下次程序启动的时候可以读取本地文件加载数据。

我们还想在每次程序进入后台的时候存储专辑数据。看起来现在这个功能并不是必须的，但是如果以后我们加了编辑功能，这样做还是很有必要的，那时我们肯定希望确保新的数据会同步到本地的归档文件。

因为我们的程序通过 `LibraryAPI` 来访问所有服务，所以我们需要通过 `LibraryAPI` 来通知 `PersistencyManager` 存储专辑数据。

在 `LibraryAPI` 里添加存储专辑数据的方法：

```swift
func saveAlbums() {
    persistencyManager.saveAlbums()
}
```

这个方法很简单，就是把 `LibraryAPI` 的 `saveAlbums` 方法传递给了 `persistencyManager` 的 `saveAlbums` 方法。

然后在 `ViewController.swift` 的 `saveCurrentState` 方法的最后加上：

```swift
LibraryAPI.sharedInstance.saveAlbums()
```

在 `ViewController` 需要存储状态的时候，上面的代码通过 `LibraryAPI` 归档当前的专辑数据。

运行一下程序，检查一下没有编译错误。

不幸的是似乎没什么简单的方法来检查归档是否正确完成。你可以检查一下 `Documents` 目录，看下是否存在归档文件。如果要查看其他数据变化的话，还需要添加编辑专辑数据的功能。

不过和编辑数据相比，似乎加个删除专辑的功能更好一点，如果不想要这张专辑直接删除即可。再进一步，万一误删了话，是不是还可以再加个撤销按钮？
