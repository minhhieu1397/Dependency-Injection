## Dependency injection
- Dependency injection (DI) là một kỹ thuật lập trình giúp tách một class độc lập với các biến phụ thuộc.
- Các class sẽ không phụ thuộc trực tiếp lẫn nhau mà thay vào đó chúng sẽ liên kết với nhau.
- Nhiệm vụ DI Tạo các đối tượng, hiểu lớp nào sẽ cần những đối tượng nào, cung cấp cho lớp đó toàn bộ các đối tượng đó. Nếu có thay đổi với đối tượng ta sẽ không cần quan tâm đến các class sử dụng đối tượng đó, mà ta chỉ cần quan tâm đến sự thay đổi của đối tượng đó thôi.
- Các phụ thuộc sẽ được truyền vào 1 class thông qua constructor của class đó :

	public function __construct(TimesheetRepository $timesheetRepository)
	    {
	        $this->timesheetRepository = $timesheetRepository;
	    }
- Hàm này ta truyền phụ thuộc TimesheetRepository vào trong class TimesheetService thông qua constructor của class TimesheetService
- Trong các phụ thuộc đều có function construct để truyền phụ thuộc. Do sử dụng constructor injection: các biến phụ thuộc được cung cấp thông qua một hàm tạo lớp.
- Ở trong Timesheet có 3 phụ thuộc TimesheetController->TimesheetService ở trong TimesheetController gọi đến thằng TimesheetService

public function __construct(TimesheetService $timesheetService)

    {

        $this->timesheetService = $timesheetService;

    }
- TimesheetService->TimesheetRepository, thằng TimesheetService gọi đến thằng TimesheetRepository:

	public function __construct(TimesheetRepository $timesheetRepository)

    {

        $this->timesheetRepository = $timesheetRepository;

    }
- TimesheetRepository  làm việc với Model để xử lí dữ liệu. Xong trả kết quả về thằng Controller:

	public function __construct(Timesheet $model)

    {

        $this->model = $model;

    }
- Sử dụng:  

	public function index() // trong TimesheetController

    {

        $timesheets = $this->timesheetService->index();// Thằng $timesheet bằng kết quả trả về thằng index trong timesheetService
     
        return view('timesheet.view', ['timesheets' => $timesheets]);

    }

    public function index() // Trong timesheetService

	{

		return $this->timesheetRepository->all(); // Hàm này sẽ trả về kết quả thằng all trong timesheetRepository

	}

	public function all() // Trong timesheetRepository

    {

    	return $this->model->all(); // Thằng này trả về kết quả  trong model Timesheet lấy ra toàn bộ dữ liệu trong bảng Timesheet

    }
