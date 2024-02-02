## 스트림과 anyMatch 사용:

anyMatch는 스트림의 요소 중 하나라도 주어진 조건과 일치하는지 확인합니다.
postReports.stream().anyMatch(report -> report.getPostId() == post.getPostId())는 Post의 postId와 동일한 PostReport가 있는지 확인합니다.
시간 복잡도는 O(n * m)이며, 여기서 n은 posts의 크기이고 m은 postReports의 크기입니다.

        List<Post> posts = // your list of posts
        List<PostReport> postReports = // your list of post reports

        posts.forEach(post -> {
            // Check if there is any PostReport with the same postId
            boolean isReported = postReports.stream()
                    .anyMatch(report -> report.getPostId() == post.getPostId());

            // Update isReported field of the Post accordingly
            post.setIsReported(isReported ? 1 : 0);
        });



## Map 사용:

postReports를 Map으로 변환하면 containsKey 메서드를 사용하여 바로 조회할 수 있습니다.
시간 복잡도는 O(n + m)입니다. 여기서 n은 posts의 크기이고 m은 postReports의 크기입니다.


        List<Post> posts = // your list of posts
        List<PostReport> postReports = // your list of post reports

        // Convert PostReport list to Map for efficient lookup
        Map<Integer, PostReport> reportMap = postReports.stream()
                .collect(Collectors.toMap(PostReport::getPostId, report -> report));

        // Update isReported field of the Post based on the presence in the reportMap
        posts.forEach(post -> post.setIsReported(reportMap.containsKey(post.getPostId()) ? 1 : 0));
