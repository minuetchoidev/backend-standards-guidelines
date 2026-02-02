# API 설계 및 네이밍 규칙

이 문서는 시스템의 보안 강화 및 일관된 인터페이스 제공을 위한 API 설계 표준을 정의합니다.

<br>


## 1. HTTP Method 보안 원칙
- **전체 API POST 메서드 단일화**: 보안 정책에 따라 모든 API 호출은 [POST 메서드 사용을 원칙](../appendix/post_only.md)으로 합니다.
- **사유**: URL 쿼리 파라미터(Query String)를 통한 데이터 노출 방지 및 모든 요청 데이터의 Body 캡슐화.

<br>

## 2. Controller 메서드 명명 표준
모든 컨트롤러 클래스 내의 함수명은 기능에 따라 아래와 같이 고정합니다.

| 기능 | 메서드 명 | 비고 |
| :--- | :--- | :--- |
| **목록 조회** | `list` | 페이징 및 필터 조건 포함 |
| **상세 조회** | `info` | 고유 식별자를 통한 단건 조회 |
| **수정** | `modify` | 정보 업데이트 |
| **삭제** | `remove` | 데이터 삭제 (Soft/Hard Delete 공통) |
| **엑셀 다운로드** | `listForExcel` | 목록 데이터의 엑셀 파일 추출 |
| **등록** | `create` | 신규 데이터 생성 |

<br>

## 3. 구현 샘플 (Java)

### 3-1. Controller (Presentation Layer)

모든 메서드는 @PostMapping 을 사용하며, 명명 규칙 (list, info, modify, remove, listForExcel)을 엄격히 준수합니다.

```java
@RestController
@RequestMapping("/api/v1/example")
@RequiredArgsConstructor
public class ExampleController {

    private final ExampleService exampleService;

    @PostMapping("/list")
    public ResultVO<List<ExampleResponseVO>> list(@RequestBody ExampleQueryVO queryVO) {
        return ResultVO.success(exampleService.list(queryVO));
    }

    @PostMapping("/info")
    public ResultVO<ExampleResponseVO> info(@RequestBody ExampleRequestVO requestVO) {
        return ResultVO.success(exampleService.info(requestVO));
    }

    @PostMapping("/modify")
    public ResultVO<Void> modify(@RequestBody ExampleModifyVO modifyVO) {
        exampleService.modify(modifyVO);
        return ResultVO.success();
    }

    @PostMapping("/remove")
    public ResultVO<Void> remove(@RequestBody ExampleRequestVO requestVO) {
        exampleService.remove(requestVO);
        return ResultVO.success();
    }

    @PostMapping("/listForExcel")
    public void listForExcel(@RequestBody ExampleQueryVO queryVO, HttpServletResponse response) {
        exampleService.listForExcel(queryVO, response);
    }
}
```

### 3-2. Service Interface (Domain Layer)

```java
public interface ExampleService {

    List<ExampleResponseVO> list(ExampleQueryVO queryVO);
    
    ExampleResponseVO info(ExampleRequestVO requestVO);
    
    void modify(ExampleModifyVO modifyVO);
    
    void remove(ExampleRequestVO requestVO);
    
    void listForExcel(ExampleQueryVO queryVO, HttpServletResponse response);
}
```

### 3-3. Service Implementation (Infrastructure Layer)

```java
@Service
@RequiredArgsConstructor
public class ExampleServiceImpl implements ExampleService {

    private final ExampleRepository exampleRepository;

    @Override
    public List<ExampleResponseVO> list(ExampleQueryVO queryVO) {
        return exampleRepository.findAll(queryVO).stream()
                .map(ExampleResponseVO::from)
                .toList();
    }

    @Override
    public ExampleResponseVO info(ExampleRequestVO requestVO) {
        return exampleRepository.findById(requestVO.getId())
                .map(ExampleResponseVO::from)
                .orElseThrow(() -> new BusinessException(ResponseCode.NOT_FOUND));
    }

    @Transactional
    @Override
    public void modify(ExampleModifyVO modifyVO) {
        ExampleEntity entity = exampleRepository.findById(modifyVO.getId())
                .orElseThrow(() -> new BusinessException(ResponseCode.NOT_FOUND));
        entity.update(modifyVO); // 도메인 모델 패턴 적용
    }

    @Transactional
    @Override
    public void remove(ExampleRequestVO requestVO) {
        exampleRepository.deleteById(requestVO.getId());
    }

    @Override
    public void listForExcel(ExampleQueryVO queryVO, HttpServletResponse response) {
        List<ExampleResponseVO> data = this.list(queryVO);
        excelUtil.download(data, "example_list.xlsx", response);
    }
}
```