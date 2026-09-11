(ns musubi.social-test
  (:require [clojure.test :refer [deftest is]]
            [musubi.methods.social :as social]
            [musubi.cells.social-post.state-machine :as machine]))

(deftest social-publication-remains-dry-run
  (let [post (social/draft-observation-post "ceremony" "observed" ["cid:a" "cid:b"])]
    (is (= ":dry-run" (get post ":post/status")))
    (is (false? (get post ":post/server-held-key")))
    (is (thrown? Exception (social/build-live post)))))

(deftest publication-state-machine-enforces-gates
  (is (= machine/phase-drafted
         (get-in (machine/transition-to-drafted
                  {"subject" "ceremony" "sources" ["cid:a" "cid:b"]})
                 ["cell_state" "phase"])))
  (is (= machine/phase-refused
         (get-in (machine/transition-to-drafted
                  {"subject" "ceremony" "sources" ["cid:a"]})
                 ["cell_state" "phase"]))))
