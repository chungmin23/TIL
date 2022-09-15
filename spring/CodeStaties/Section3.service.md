## Member 클래스 역할

 DTO가 API 계층에서 클라이언트의 Request Body를 전달 받고 클라이언트에게 되돌려 줄 응답 데이터를 담는 역할을 한다면, `Member` 클래스는 **API 계층에서 전달 받은 요청 데이터를 기반으로 서비스 계층에서 비즈니스 로직을 처리하기 위해 필요한 데이터를 전달 받고, 비즈니스 로직을 처리한 후에는 결과 값을 다시 API 계층으로 리턴해주는 역할**을 합니다.

member 클래스 

~~~java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class Member {
    private long memberId;
    private String email;
    private String name;
    private String phone;
}
~~~



## 매퍼 

 DTO 클래스를 entity 클래스를 서로 변환해준다

~~~java
@Component  // (1)
public class MemberMapper {
		// (2) MemberPostDto를 Member로 변환
    public Member memberPostDtoToMember(MemberPostDto memberPostDto) {
        return new Member(0L,
                memberPostDto.getEmail(), 
                memberPostDto.getName(), 
                memberPostDto.getPhone());
    }

		// (3) MemberPatchDto를 Member로 변환
    public Member memberPatchDtoToMember(MemberPatchDto memberPatchDto) {
        return new Member(memberPatchDto.getMemberId(),
                null, 
                memberPatchDto.getName(), 
                memberPatchDto.getPhone());
    }

    // (4) Member를 MemberResponseDto로 변환
    public MemberResponseDto memberToMemberResponseDto(Member member) {
        return new MemberResponseDto(member.getMemberId(),
                member.getEmail(), 
                member.getName(), 
                member.getPhone());
    }
}
~~~

memberController 를 mapper 적용

~~~java
@RestController
@RequestMapping("/v4/members")
@Validated
public class MemberController {
    private final MemberService memberService;
    private final MemberMapper mapper;

		// MemberMapper DI
    public MemberController(MemberService memberService, MemberMapper mapper) {
        this.memberService = memberService;
        this.mapper = mapper;
    }

    @PostMapping
    public ResponseEntity postMember(@Valid @RequestBody MemberPostDto memberDto) {
				//  매퍼를 이용해서 MemberPostDto를 Member로 변환
        Member member = mapper.memberPostDtoToMember(memberDto);

        Member response = memberService.createMember(member);

				// 매퍼를 이용해서 Member를 MemberResponseDto로 변환
        return new ResponseEntity<>(mapper.memberToMemberResponseDto(response), 
                HttpStatus.CREATED);
    }

    @PatchMapping("/{member-id}")
    public ResponseEntity patchMember(
            @PathVariable("member-id") @Positive long memberId,
            @Valid @RequestBody MemberPatchDto memberPatchDto) {
        memberPatchDto.setMemberId(memberId);

				//  매퍼를 이용해서 MemberPatchDto를 Member로 변환
        Member response = 
              memberService.updateMember(mapper.memberPatchDtoToMember(memberPatchDto));

        //  매퍼를 이용해서 Member를 MemberResponseDto로 변환
        return new ResponseEntity<>(mapper.memberToMemberResponseDto(response), 
                HttpStatus.OK);
    }

    @GetMapping("/{member-id}")
    public ResponseEntity getMember(
            @PathVariable("member-id") @Positive long memberId) {
        Member response = memberService.findMember(memberId);

				//  매퍼를 이용해서 Member를 MemberResponseDto로 변환
        return new ResponseEntity<>(mapper.memberToMemberResponseDto(response), 
                HttpStatus.OK);
    }

    @GetMapping
    public ResponseEntity getMembers() {
        List<Member> members = memberService.findMembers();

				// (7) 매퍼를 이용해서 List<Member>를 MemberResponseDto로 변환
        List<MemberResponseDto> response =
                members.stream()
                        .map(member -> mapper.memberToMemberResponseDto(member))
                        .collect(Collectors.toList());

        return new ResponseEntity<>(response, HttpStatus.OK);
    }

    @DeleteMapping("/{member-id}")
    public ResponseEntity deleteMember(
            @PathVariable("member-id") @Positive long memberId) {
        System.out.println("# delete member");
        memberService.deleteMember(memberId);

        return new ResponseEntity(HttpStatus.NO_CONTENT);
    }
}
~~~



