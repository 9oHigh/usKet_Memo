**Memo Project ๐๐ปโโ (2021.11.9 ~)**

1. ๊ธฐ๋ณธ์ ์ธ ๋ ์ด์์ ๋ฐ ์ฐ๊ฒฐ  - 11.9 
2. ์ธํธ๋ก, Realm - 11.10
3. ์ค๋ฅ๊ฐ ๋๋ฌด ๋ง์ / ์์น๋ฐ ์ปจํธ๋กค๋ฌ ๊ตฌํ ํด์ผํจ - 11.12 
4. ๋ ์งํฌ๋งท, ํค๋ณด๋, ๊ณต์ ๊ธฐ๋ฅ, ๋ฐฑ๋ฒํผ์ ๋ฉ๋ชจ์ ์ฅ, ๋๊ฐ์ ์นผ๋ผ์ผ๋ก ์ ์ฅ ๋ฏธ๋ฐ์ - 11.15

<br></br>

<div align = "center">
  
  ![แแณแแณแแตแซแแฃแบ 2021-11-15 แแฉแแฎ 11 27 44](https://user-images.githubusercontent.com/53691249/141798699-0383b331-481d-4540-8422-b6fb10269de1.png)
  
</div>

### 1.MVC ๋์์ธ ํจํด
- ์ด๋ ดํ์ด ๋ค์๋ MVC ๋์์ธ ํจํด์ด ๊ธฐ์ต์ด๋ ํ ๋ฒ ์ ์ฉํด๋ณด๊ณ ์ ํด๋๊ด๋ฆฌ๋ฅผ ํด๋ณด์์ผ๋ ์ ํํ๊ฒ ํ๊ณ  ์๋์ง ์์ง์ ๋ชจ๋ฅด๊ฒ ๋ค.

**[ MVC ]**
- Model : Realm table๊ณผ MemoviewController์์ ์ฌ์ฉํ๋ ํ๋กํ ์ฝ์ ์ ์ฅ
- View : Storyboard ๋ฐ cell
- Controller : Memo, Walkthrough,Editor / controller์ Extension

**๊ฐ์ธํ๋ก์ ํธ์์ ํ์คํ๊ฒ ์ตํ๋๊ณ  MVVM์ ๋ํด์ ํ์ตํด๋ณด์.**
<br></br>
### 2.๋ฐ์ดํฐ ์ ๋ฌ

```swift
protocol SendDataDelegate {
    //๊ธฐ๋ณธ
    func textData(title : String,content : String)
    //๊ณ ์  + ๋ฉ๋ชจ
    func textDataTag(title : String,content : String,tagging : Int)
    //์์น
    func textDataId(title : String,content : String,id : ObjectId)
    
}
```
- ์๋นํ ์ ๋ฅผ ๋จน์ธ ๋ถ๋ถ์ด๋ค. 
- ์๋ก์ด ๋ฉ๋ชจ๋ฅผ ์์ฑ์์๋ ๊ฐ์ฅ ์์์ textData๋ฅผ ์ด์ฉ
- ๊ณ ์ ๋์ด ์๊ฑฐ๋ ๋ฉ๋ชจ์ ์์ ๊ฒฝ์ฐ์๋ textDataTag๋ฅผ ์ด์ฉ
- ๊ฒ์์์๋ textDataId๋ฅผ ์ด์ฉํ๋๋ฐ ์ด ๊ฒฝ์ฐ์๋ ์น์์ด ํ๋์ด๊ธฐ ๋๋ฌธ์ indexPath.section/row๋ฅผ ์ด์ฉํ๊ธฐ ๊น๋ค๋ก์ id๊ฐ์ ์ด์ฉํด ์ ๋ฌํ๋ค.
<br></br>
### 3.NumberFormt
```swift
func numberFormatter(number: Int) -> String {
        
        let numberFormatter = NumberFormatter()
        numberFormatter.numberStyle = .decimal
        
        return numberFormatter.string(from: NSNumber(value: number))!
    }
```
- Date๋ง๊ณ  ์ซ์๋ Format์ ์ ํด์ค์ ์๊ฒ ๋ง๋ค์ด์ฃผ๋ ํจ์
<br></br>
### 4.๋ ์ด๋ธ์ ์ผ๋ถ๋ฅผ ์ปค์คํฐ๋ง์ด์ง
```swift
 let attributtedString = NSMutableAttributedString(string: cell.titleLabel.text!)
            attributtedString.addAttribute(NSAttributedString.Key.foregroundColor, value: UIColor.red, range: (cell.titleLabel.text! as NSString).range(of:"\(searchController.searchBar.text!)"))
            cell.titleLabel.attributedText = attributtedString
```
<br></br>
### 5.LeadingSwipe ํจ์
```swift
func tableView(_ tableView: UITableView, leadingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {}
```
- ๊น๋จน์ง ๋ง์๐
<br></br>
### 6.์ ํ๋ฆฌ์ผ์ด์ ์ฒ์ ๊ตฌ๋์ ํ์ธ ํจ์
```swift
//์ฒ์์ธ์ง ์๋์ง ํ์ธ
  func firstLogInCheck(){
      //๋๋ฒ์ด์ ์คํ
      if userDefaults.bool(forKey: "FirstLogIn") { return }
      //์ฒ์ ์คํ
      userDefaults.set(true,forKey: "FirstLogIn")

      //์ฒ์์ด๋ฏ๋ก ํํ ๋ฆฌ์ผ ์๋ด
      let storyboard = UIStoryboard(name: "walkthrough", bundle: nil)
      let vc = storyboard.instantiateViewController(withIdentifier: "walkthrough") as! WalkthroughViewController
      //overFullScreen์ด์ฌ์ผ ๋ผ์ดํ์ฌ์ดํด ์๊ฒน์น๋ค!
      vc.modalPresentationStyle = .overFullScreen

      self.present(vc, animated: true, completion: nil)
  }
```    
- UserDefaults๋ฅผ ์ด์ฉํด ํค์ Bool ํํ๋ก ์ ์ฅํด๋๊ณ  ์ ํ๋ฆฌ์ผ์ด์์ ํค๋ฉด ํ์ธํ๋ค.
- ๋ค๋ง Appdelegate๋ฅผ ํ์ฉํ๋ฉด ํ๋ฒ๋ง ํ์ธํ  ์ ์๋ค๋ ๊ธ์ ๋ณธ ๊ฒ ๊ฐ์ ํ์ธํด๋ณผ ํ์๊ฐ ์๋ค.
<br></br>
### 7.๊ณ ์ ๋ ๋ฉ๋ชจ์ ๋ฉ๋ชจ๋ฅผ ๋ถ๋ฆฌํ๊ธฐ ์ํ ํ๋กํผํฐ
```swift
 //๊ณ ์ ๋ ๋ฉ๋ชจ์ ๋ฉ๋ชจ๋ก ๋ถ๋ฆฌํ๊ธฐ ์ํ ํ๋กํผํฐ
    var classifications : [String] {
        return Set(Works.value(forKeyPath: "classification") as! [String]).sorted()
    }
```
- indexPath.section / indexPath.row๋ฅผ ์ด์ฉํด์ ๋ถ๋ฆฌ๋๊ฒ ๋ณด์ฌ์ฃผ๋ ๊ณผ์  ์์ฒด๋ ์ฌ์ ์ผ๋ indexPath.row๊ฐ ๊ฒน์น๊ฒ ๋์ด ๊ณ ์ ๋ ๋ฉ๋ชจ์ ๊ฐ ํน์ ๋ฉ๋ชจ์ ๊ฐ์ด ํผ๋๋์ด ํ์๋๊ฑฐ๋ ์ค๋ฅ๊ฐ ๋ฐ์ํ์๋ค.
- Realm์ classification์ด๋ผ๋ ๋ณ์๋ฅผ ์ถ๊ฐํ๊ณ  String ํ์์ผ๋ก '๊ณ ์ ๋ ๋ฉ๋ชจ'์ '๋ฉ๋ชจ'๋ก ๋๋๊ณ  ์์ ํ๋กํผํฐ๋ฅผ ํ์ฉํ์ฌ ํ์ด๋ธ ๋ทฐ์ ๋ณด์ฌ์ฃผ์๋ค.
- ์๋นํ ์ค๋์๊ฐ ์ ๋จน์๋ค.. ์์ง๋ ๋ถ๋ค๋ถ๋คํ๋ค..

<div align="center">
  
  <h4> [์ค๊ฐํ] : ์ฒ์์ผ๋ก ํด๋ณด๋ ๊ฐ๋จํ ํ๋ก์ ํธ ์์ง๋ง ์์ง๋ ๊ตฌํํ์ง ๋ชปํ ๊ธฐ๋ฅ๋ค์ด ์๊ณ  ๋ ์ค๋ฅ๊ฐ ์๋ค. ์ถํ์ ๊ฐ์ธ ํ๋ก์ ํธ๊ฐ ๋ง๋ฌด๋ฆฌ๋๊ฐ๊ฑฐ๋ ์๊ฐ์ด ๋ ๋๋ง๋ค ์์ ํด์ผ ๊ฒ ๋ค๐๐ปโโ๏ธ </h4>

</div> 
<br></br>
<div align="center">
  
  <h4> ๊ธฐ๋ฅ๊ตฌํ ์์ ๋ฐ ์คํฌ๋ฆฐ์ท</h4>

</div> 
<div align = "center">
  <img src="https://user-images.githubusercontent.com/53691249/141805876-11c52292-c024-45ee-ae45-fdd1fc62cf3d.png" width="30%" height="30%">
<!--  ![Simulator Screen Shot - iPhone 12 Pro Max - 2021-11-15 at 21 31 25](https://user-images.githubusercontent.com/53691249/141805876-11c52292-c024-45ee-ae45-fdd1fc62cf3d.png){height=300px width=200px} -->
  
</div>
<br></br>
<div align = "center">
  
  ![ezgif com-gif-maker](https://user-images.githubusercontent.com/53691249/141784974-601c45b3-05d0-4d17-90c6-8fde4686fe2a.gif)
  
</div>


