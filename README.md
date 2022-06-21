# Class-Base

# BaseVM
```
import RxSwift
class BaseVM{

    
    let baseAlert: BaseAlert
    init(){
        baseAlert = BaseAlert(message: PublishSubject<String>(), loadding: PublishSubject<Bool>())
    }
    
    let bag = DisposeBag()
    
    func loadding(showAlert:Bool ){
        baseAlert.loadding.onNext(showAlert)
    }
    
    func messaging(stringMessage:String){
        baseAlert.message.onNext(stringMessage)
    }
    
    
}

struct BaseAlert{
    let message:PublishSubject<String>
    let loadding:PublishSubject<Bool>
}
```


# BaseVC
```
import RxSwift
class BaseViewController<VM:BaseVM>:UIViewController{
    var viewModel:VM!
    
    
    var bag = DisposeBag()
    
    
    var showIndiators = UIActivityIndicatorView()
    
    func setUpUI(){
        
    }
    func setUpViewModel(){
        
    }
    
    func setUpData(){
        
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        setUpUI()
        setUpViewModel()
        setUpData()
        
    }
    
    func setUpBinding(){
        viewModel.baseAlert.message
            .observe(on: MainScheduler.instance)
            .subscribe(onNext: { element in
                self.alert(message: element)
                
            })
        
        viewModel.baseAlert.loadding
            .observe(on: MainScheduler.instance)
            .subscribe(onNext: { element in
                
            })
    }
    
}

---------------------------------- Notes-----------------------------
BaseVM lấy ở BaseVM


# đi theo luồng từ trên xuống


# setUpUI sử dụng refreshControl để reload  trong table view -> chạy đầu


# setUpViewModel -> chạy thứ 2 để setupViewModel thì mới làm ăn được
thứ 1 để khởi tạo viewModel trong từng ViewController
thứ 2 để nhận alert hoặc message để thông báo
thứ 3 là để gửi các onNext tới các ViewController của viewModel hiện tại
thứ 4 là sử dụng bind data từ các nguồn phát onNext vào rx items trong tableview , vào các đối tượng muốn nhận dữ liệu
thứ 5 là thao tác với delegate của table view (modeSelected,itemSelected,itemDeselected,refreshControl....)
thứ 6 là sử dụng viewModel để load API trong class viewModel
thứ 7 là cập nhật UI vì tác dụng thứ 4 sẽ bind data từ viewModel qua bên ViewController này

# setUpData
thứ 1 là dùng viewModel.inReload.onNext(()) để phát tín hiệu để thằng viewModel nhận và xử lý logic nghiệp vụ
```



